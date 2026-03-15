# Network Model

> **TCP/IP Reference Model** (evolved from **Open System Interconnection Model OSI**)
> Below I divide network access layer of **TCP/IP Reference Model** into data link layer and physical layer

**Application Layer**: HTTP SMTP RTP DNS FTP (Application + Presentation + Session Layers in OSI)
PDU Protocol Data Unit: message
数据用什么格式去满足不同应用程序的需求
**[[Transport Layer]]**: TCP UDP - Protocols/Ports
PDU: TCP: segment; UDP: datagram
数据用什么格式去满足不同通讯形式的需求
**[[Network Layer]]**: IP ICMP - IP address
PDU: packet
数据如何在多个网络上的传输（路由器以上）
**[[Data Link Layer]]**: DSL SONET 802.11 Ethernet - MAC address
PDU: frame
数据如何在一个网络上的传输（路由器以下）
**Physical Layer**
PDU: bit stream
何种信号表示比特，何种介质传输比特
# Structures
PAN personal area network: vicinity
LAN local area network: building
MAN metropolitan area network: city
WAN wide area network: country
Internet the global network: planet
  
Bluetooth is a PAN standard
WLAN Wireless LAN with 802.11
Wired LAN with switched Ethernet
ISP: internet service provider, ISP network is a WAN through transmission lines
VPN Virtual Private Network is a WAN through links on the internet
  
**Physical Connection Methods:**
Ethernet
Wi-Fi wireless fidelity
  
**House Wi-Fi:**
Media Access Control address MAC address (physical address)
猫（光猫）： 信号转换 → 路由器： 将转换后的信号分配给正确的电脑
modem (ONT: Optical Network Terminal) → router (access point + router)
# Performance
**bitrate**: bits per second (bps), kilobits per second (kbps), megabits per second (Mbps)
Note: Unlike 1KB = 1024B, bitrate has 1kbps = 1000bps, 1Mbps = 1000kbps
**bandwidth**: highest bitrate
**delay**: processing delay + transmission delay + propagation delay