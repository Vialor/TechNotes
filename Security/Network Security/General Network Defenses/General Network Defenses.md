# Challenges

| 特性          | 网络边界（Network Perimeter） | 内部网络（Internal Network） |
| ----------- | ----------------------- | ---------------------- |
| **位置**      | 内部网络和外部网络（互联网）之间的交界处    | 组织或公司的内部局域网            |
| **主要目的**    | 阻挡外部攻击、防止未授权流量进入        | 防范内部威胁，限制内部网络的访问和传播    |
| **常见工具和技术** | 防火墙、VPN、IDS/IPS、反向路径过滤  | 内部防火墙、VLAN分段、HIDS、端点防护 |
# Network Security Tools
[[VPN Virtual Private Network]]
**Firewall**
	Packet filtering firewall
	Stateful inspection firewall
	Application proxy firewall (Most sophisticated)
	Circuit-level proxy firewall (proxy on transport layer)
**IDS Intrusion detection system**: SNORT
	NIDS Network-based IDS; HIDS Host-based IDS
	How: Anomaly detection(learn over time), Pattern matching(catch known attacks)
	Data Drift: Distribution of the features of data changes over time rendering learned detection model less effective
**IPS Intrusion prevention system**
	NIPS Network-based IPS; HIPS Host-based IPS; WIPS Wireless IPS
	NBA Network Behavior Analysis

|           | Firewall                              | IPS                                                                                | IDS                                                                      |
| --------- | ------------------------------------- | ---------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| Placement | inline                                | inline, behind firewall                                                            | non-inline, through port span or tap, usually behind firewall            |
| Mechanism | filter traffic based on IPs and ports | look for traffic patterns or signature of attacks and prevent attacks on detection | look for traffic patterns or signature of attacks and alert on detection |

**Honeypot**: a simulation of a fake system that lures attacks, which can then be analyzed
	**Server Honeypot**: pretending to be a vulnerable server
	**Client Honeypot**: pretending to be a gullible client
**(Forward) Proxy**: a middle service that send data to the servers for the clients; as such, it can monitor and protect outgoing traffic
**Reverse Proxy**: a middle service that receive data from the clients for the servers; as such, it can monitor and filter incoming traffic