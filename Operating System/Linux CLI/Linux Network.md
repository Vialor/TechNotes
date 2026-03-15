```bash
# my ip info
ifconfig
ipconfig
ip a

# 查看网络接口的 IP 地址
ip addr show
# 查看路由表
route
ip route show

# Check ports
lsof -i :8080
netstat -ano | findstr :8080

# Download
curl [options...] <url>
	-s # silent mode
	-H header
	-X action: GET CTEATE POST DELETE
	-o output file
	-O output file with remote name
	-C - 断点续传，从上次中断的地方继续下载
wget [options...] <url>
	-r --recursive
	-np --no-parent
	-R --reject
		-R "index.html*"
	--cut-dirs=X
	-e robots=off
```