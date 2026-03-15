> send packet to the right computer
# IP Internet Protocol Address
## IPv4: version 4 of IP  
a 32-bit address, 0.0.0.0 to 255.255.255.255 (4 Octets: 4 * 2^8)  
has two parts: Network and Host  
host bits all 1s is a broadcast address, host bits all 0s is the network address, they are reserved  

> The history: classful network ⇒ add a subnet layer to classful network ⇒ CIDR: no more class ABCDE

**Classful Network:**
class A: begin with 0, 0.0.0.0 to 127.255.255.255, 8-bit network 24-bit host
class B: begin with 10, 128.0.0.0 to 191.255.255.255, 16-bit network 16-bit host
class C: begin with 110, 192.0.0.0 to 223.255.255.255, 24-bit network 8-bit host
class D: begin with 1110, 224.0.0.0 to 239.255.255.255, multicast address
class E: begin with 11110, unused
**Subnet Mask**: divides IP into network half and host half, network half contains both the network bits and the subnet bits
**CIDR Classless Inter-Domain Routing:**
CIDR notation: 192.4.16/20 means the first 20 bits are network bits, the rest host bits
Prefixes can have any length from 2 to 32.

**IPv4 Scope**:
	**Public IP Addresses:** addresses that are publicly registered on the internet, a public IP is necessary to connect to the internet.
	**Private IP Addresses**: addresses that are NOT publicly registered on the internet
## IPv6: version 6 of IP
a 128-bit hexadecimal address, :: to FFFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF (8 * 2^16)

**IPv6 Scope**:
	**Link-Local**: used on a single network link (under the router)
		`FE80::/10` + 54 bits + 64-bit interface id
	**Site-Local**: use on a local network (across the routers within a organization)
		`FC00::/7` + 4-bit global id + 16-bit subnet id + interface id
	**Global Unicast**: unique in the whole internet
## IPv4-IPv6 Compatibility
**Dual Stack**: a device supports both IPv4 and IPv6 protocols, so it can process data from both IPv4 only devices and IPv6 only devices
### NAT: Network address translation:
> All but a series of disastrous outcomes of a bad design choice: IPv4

What: A service used by routers to translate a set of **private IP addresses** to a set of **public IP addresses**, and a set of **public IP addresses** to **private IP addresses**.  
Why: NAT works with private IP addresses preserve the limited amount of IPv4 public addresses  
**NAPT Network Address and Port Translation  
  
**A service used by routers to translate a set of private IP addresses** to a set of **public IP addresses**, and a set of to **private IP addresses**.
Private Addresses and NAT are **“dying”** because IPv6 is coming.
# IP packet structure
packet: IP header + data payload
## IPv4 Header
(20+ bytes)
Version: version of IP
IHL internet header length: length of the IP header (divided by 4 bytes)
Total length: length of the IP header and data payload (of this fragment)
  
Identification: an identifying value assigned by the sender to aid in assembling the fragments of an IPv4 Datagram
Flags: flags about fragmentation
Fragment offset: the exact position of the fragment in the original IP Packet (divided by 8B)
**fragmentation** (dividing data to be sent into several IP packets) is necessary due to **Maximum Transmission Unit MTU**)
  
Time to live: how many times this packet can hop among routers
Upper layer protocol
Header checksum
  
Source of IP address
Destination of IP address
## IPv6 Header
(40+ bytes)
### Extension Header
**Next header** field indicates the next extension header right following the current header in the very same packet.
- **Hop-by-Hop Options Header**（逐跳选项头）：包含对每个节点都必须检查的信息，如路由器警报。
- **Routing Header**（路由头）：用于源路由，即指定数据包传输路径的中间点。
- **Fragment Header**（分片头）：包含分片和重组信息。
- **Destination Options Header**（目的地选项头）：为到达的数据包目的地提供可选信息。
- **Authentication Header** 和 **Encapsulating Security Payload Header**：提供数据包的安全性和完整性保护。
# IP Cast
**Unicast**: single host to single host traffic
**Multicast**: multiple devices share the same class D IP address to receive **all the traffic** as a group
**Anycast**: multiple devices share the same class D IP address to receive **a part of the traffic** as a group
**Broadcast**: single host sends traffic to all other hosts on the subnet (Layer 3 broadcast is also called directed broadcast)
directed broadcast is more powerful than local broadcast because it can speak to all hosts in the local subnets and all the hosts in foreign subnets.
**IGMP Internet Group Management Protocol**: a protocol used to set up multicast on IP networks
- Usual Class D IP Assignment: (And there is a way to compute MACs from these IPs)
    224.0.0.1 All systems on this subnet
    224.0.0.2 All routers on this subnet
    224.0.0.5 OSFP routers