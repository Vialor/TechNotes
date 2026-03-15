# Types of VPN
## Site-to-Site VPN
场景：两站点地点固定，链接不间断
两地各有一个VPN集线器/VPN服务器/VPN网关，集线器处加密解密。在第三者看来网络数据包就是两个集线器之间的加密对话。
Encryption: IPSec
## Remote Access VPN
场景：客户端地点不固定，站点地点固定
全隧道：远程用户的所有网络流量都通过 VPN 隧道传输到站点端的网络，再由公司的 VPN 网关作为中间人代表用户发送请求
半隧道：访问公司内部资源的流量会通过 VPN 隧道传输，访问互联网的流量直接通过用户的本地网络（即家庭网络或公共网络）访问
Encryption: SSL/TLS, IPSec