**mitmproxy**：一组工具，为HTTP/1、HTTP/2、WebSockets协议，提供了可交互的、SSL/TLS解密的中间人代理
	处理不了UDP，以及**SSL Pinning**：客户端“只信任指定证书”，从而阻止中间人解密 HTTPS
同一个功能，三个入口：
- **`mitmproxy`**：终端里的**交互式界面**
- **`mitmweb`**：浏览器里的**交互式界面**
- **`mitmdump`**：纯命令行/脚本化/批处理入口
	`-n`离线模式，读取历史文件，不启动代理监听端口
		`mitmdump -ns script.py -r infile -w outfile "filter expression"`
	`-s`在线模式，默认，启动代理实时监测客户端
		`mitmdump -s mitm_hooks.py -w outfile --listen-host 127.0.0.1 --listen-port 8080`
		`mitmdump -s mitm_hooks.py -w outfile "~d remote-server.com"`
# Modes
```bash
**Regular Mode**: 客户端手动配置HTTP代理到mitm监听地址，因此客户端知道中间人的存在
**Upstream Mode**: 原本客户端需要一个代理来访问服务器，mitmproxy 自己作为代理，同时再把请求转发到上游代理：Client → mitmproxy → 上游代理 → Server
mitmdump --ssl-insecure --set upstream_auth=USER:PASSWORD --mode upstream:http://proxyca.huawei.com:8080 -s mitm_hooks.py -w outfile

**Local Mode**: 操作系统按进程名或pid劫持，客户端不知道mitm存在
mitmweb --mode local:python.exe
mitmweb --mode local:12345

**Reverse Mode**: mitmproxy 伪装成服务器，客户端连它（客户需要把url改成mitm地址），它再转发到真实服务器，需要mitm不通过代理直连服务器
mitmweb --mode reverse:https://example.com

**Transparent Mode**: 通过网络层重定向，把流量强制送进 mitmproxy，流量路径 Client → (iptables/pf) → mitmproxy → Server。相比Local优势在于跨进程捕捉、以及捕捉经过本设备的同网络下其他设备的流量，但更笨重。
可以抓整个局域网流量

**WireGuard Mode**（VPN 模式）：mitmproxy 充当 VPN 服务器，客户端主动把流量发过来，可以跨设备和进程
mitmweb --mode wireguard
手机安装 WireGuard App，就可以把流量发给电脑
```

# Hooks
```python
from mitmproxy import http

class AddHeader:
    def request(self, flow: http.HTTPFlow):
        flow.request.headers["X-Debug"] = "1"

    def response(self, flow: http.HTTPFlow):
        flow.response.headers["X-Proxy"] = "mitmproxy"

addons = [
    AddHeader()
]
```

```python
import json
from mitmproxy import http

def request(flow: http.HTTPFlow) -> None:
    if "httpbin.org" not in flow.request.pretty_host:
        return

    if flow.request.method.upper() != "POST":
        return

    content_type = flow.request.headers.get("content-type", "")
    if "application/json" not in content_type.lower():
        return

    try:
        text = flow.request.get_text()
        data = json.loads(text)
    except Exception as e:
        print("JSON parse failed:", e)
        return

    if isinstance(data, dict):
        if data.get("name") == "alice":
            data["name"] = "bob"

        data["added_by_mitm"] = True

        flow.request.set_text(json.dumps(data, ensure_ascii=False))
        print("Modified request JSON:", data)
```