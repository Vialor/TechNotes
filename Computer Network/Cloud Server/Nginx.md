# Nginx in Shell
```bash
nginx -v
nginx -t # check error in nginx.conf
nginx -s reload # reload after change in nginx.conf
nginx -s stop # stop the Nginx server
v # start the server if it's not running
```
# Web服务
主配置文件：`/etc/nginx/nginx.conf`
```
pid /var/run/nginx.pid # contain process ID (PID) of the running Nginx master process
events {}
http {
  include /etc/nginx/mime.types # set correct file types
  include conf.d/*.conf # config settings under conf.d will be pasted here
  server {
    listen 80;
    server_name localhost;
    # location <operation param> <path> {}
    # location = <path>
    # location = <path> exact match
	# location ^~ <path> # 优先前缀匹配，优先：regex会不会抢走匹配权
    # location ~ <path> regex match
    # location ~* <path> regex match ignoring cases
    location <relative_path> { # 普通前缀匹配
	    root <root_path>
    }
    location /temp {
	    root 307 /app/index.html # 重定向
    }
    rewrite /temp /app/index.html # 重写
  }
}
```
location优先级从高到底：
(`location =`) > (`location 完整路径`) > (`location ^~ 路径`) > (`location ~,~* 正则顺序`) > (`location 部分起始路径`) > (`/`)
## Reverse Proxy & Load Balancing
`/etc/nginx/conf.d/default.conf`

```
# 策略1：轮询（默认）
upstream backend-servers1 {
   server localhost:8081;
   server localhost:8082;
}

# 策略2：权重（按比例循环，服务器性能不均时考虑）
upstream backend-servers2 {
    server localhost:8081 weight=1;
    server localhost:8082 weight=3;
    server localhost:8083 weight=4 backup; # 8081,8082都挂时
}

# 策略3：ip_hash（确保同一个ip访问的是同一个服务器，确保session连续性）
upstream servers3 {
    ip_hash;
    server localhost:8080;
    server localhost:8081;
}

server {
  listen 80;
  server_name localhost;

  location / {
    proxy_pass http://backend-servers1;
  }
}
```