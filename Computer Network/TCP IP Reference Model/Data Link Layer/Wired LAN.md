**Bus Topology**: all hosts connect to the same cable
**Star Topology** with **Hub 集线器**: an outdated *physical layer* hardware used in the ethernet to allow multiple hosts and other hubs to connect to a single port

**Switch 交换机**: a *data-link layer* hardware used in the ethernet to allow multiple hosts and other switches to connect to a single device
**CAM table** (content addressable memory): a table in **switches**; it stores the mappings *from MAC addresses to VLAN ID + port numbers* for later data transmissions
	每当交换机收到一个数据帧时，它会记录帧的**MAC addresses, VLAN ID and port numbers**，然后将这个映射关系添加到 CAM table
**Trunk**: the link between a switch and a switch or a router
	Trunk is a special link because it allows traffic from multiple VLANs

**Bridge**: a hardware interconnect multiple LANs by passing frames to each other
**Collison domain**: the domain in which, the signals that are transmitted by the devices over the network collide with each other  
  
**Switches and bridges separate collision domains, yet hubs do not.**
**Broadcast domain**: a domain that consists of all the devices that can receive a broadcast message that is sent by any other device which is present in the domain
**Broadcast, Unicast, Multicast**
	Layer 2 broadcast is also called local broadcast

**Spanning Tree Protocol STP**:
motivation: detect if there is a loop in the network
how: broadcast from a root, and see if a machine received two messages

**VLAN Virtual LAN**: Divide a network into multiple sub networks without changing its physical structure.
	VLAN is done by switches(usually) inserting before the data of a frame a label of the VLAN where the frame comes from. (802.1q) Unlike trunks, usually, a link and a port only allow frames from one VLAN. 
## **CSMA Carrier Sense Multiple Access**
**Carrier Sense**: check if there are other stations sending information on the channel before sending data (看电压摆动值是否超过阀值)  
**Multiple Access**: multiple stations on the same channel
## **CSMA/CD Carrier Sense Multiple Access/Collision Detection**

> Used in **IEEE 802.3**, which is a Wired LAN standard for ethernets  
> CSMA/CD = CSMA + detect collision while transmitting data  

**Contention Period**: 2t (end-to-end propagation delay on the bus: t)  
It is the max time to know if my frame collides  
Hence it is the amount of time for which a Host should keep on transmitting data so that if there is a collision anywhere in the channel, the signal should reach the Source Host  
**Exponential Backoff Algorithm:** waiting time is random in [0, 2^k-1], and k is the min(number of conflicts, 10); gives up and reports error when k has reached 16
![[Untitled 2.png|Untitled 2.png]]
**Full-duplex**: data can be transmitted in both directions on a signal carrier at the same time  
CSMA/CD is moot on full-duplex comminication, which is usually the modern solution.  