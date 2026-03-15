>Reference: [[Data Link Layer]]
>Tool: Yersinia

**CAM Overflow**: Macof (included in Dsniff)
	Send frames to flood the CAM tables in switches. Since switches cannot learn more mappings, switch can only broadcasts, instead of unicast those traffic that has no entry in the table yet. Since it is broadcasting, the attackers can then eavesdrop. 
*Solution*: **Port Security**: limits the MAC addresses stored for each of the ports of switches
- - -
**VLAN Hopping Attack**: attacks that involves sending frame to other VLANs
**Basic VLAN Hopping Attack**: Attacker machine spoofs as a switch and therefore is recognized as a member of all VLANS. As an result, the attacker has access to traffic in other VLANs.
*Solution*: Manually configure switch ports and specify which are trunk ports, which are access ports

**Double 802.1q Encapsulation VLAN Hopping Attack**:
	Take advantage of **Native VLAN**: a default VLAN when the frames are not labeled
	The loophole: when a frame contains a native VLAN label, switches will remove it (they don't do this for other VLAN). VLAN is introduced later, so this features exist for compatibility reason.
	Attack: Attackers send two labels, first one is the native VLAN to cheat switches. After the switches removes the first label, the second one goes to any VLAN the attackers want.
*Solution*: Do not use Native VLAN, every frame should have an explicit VLAN ID
- - -
Reference: [[DHCP Dynamic Host Configuration Protocol]]

**CHADDR DHCP Starvation Attacks**: one gobbler client could deplete the IP pools by requesting all possible IP addresses with different fake CHADDR(Client Hardware Address)

**Rogue DHCP Server Attacks**: set up a fake DHCP server to give false configurations including:
	1. wrong default gateway
	2. wrong DNS server
	3. wrong IP addresses

*Solution*: **DHCP Snooping**: switches monitor DHCP traffic and maintain the **DHCP Snooping Binding Table**, which stores: MAC, IP, IP lease time, IP Binding Type(static or dynamic), VLAN ID, Switch port(or interface)
	Switches check if CHADDR actually match the MAC of the client
	Admins set switches to know which port is trusted for DHCP server, so switches drop DHCP replies from untrusted ports
- - -
Reference: [[ARP Address Resolution Protocol]]

**ARP Spoofing**: A broadcasts for B's MAC, the attacker unicasts its MAC to A using B's IP
*Solution*:
	**Dynamic ARP Inspection DAI**: All ARP packets must match the IP/MAC in DHCP Snooping Binding Table. For non-DHCP devices, use static DHCP Snooping Binding Table.
	**IP Source Guard IPSG**: All IP packets must match the IP/MAC in DHCP Snooping Binding Table
**NDP Spoofing**: There is still NO authentication or encryption in *NDP Stateless address autoconfiguration*, and thus ARP Spoofing becomes NDP Spoofing for IPv6.
# Wi-Fi Security
Wi-Fi session establishment:
1. Probe request & response
2. Authentication request & response
3. Association request $ response
4. Data & ACK

Wi-Fi security protocols:
WEP Wired Equivalent Privacy (deprecated)
--> WPA Wi-Fi Protected Access (deprecated)
--> WPA2(WPA2-Personal, WPA2-Enterprise), WPS Wi-Fi Protected Setup
--> WPA3-Personal

## Attacking WEP
**The Passive Attack**: WEP has short Initialization vectors, so that attackers can sniff data transmitted and guess the secret key after analysis.
**Active Attack**: attackers actively send packets with different IVs, so they can gather more data and crack the secret key 
**Café Latte Attack**: - The attacker captures probe requests from devices with the secret key(such devices have connected to the Wi-Fi before) trying to connect to Wi-Fi, and respond to them pretending to be the legitimate AP. The attacker floods the client with ARP requests, and from the clients they gather enough responding data packets encrypted with the WEP key to crack the secret key.
**Evil Twin**: Rogue Wi-Fi access point appears to be a legitimate one offered in public and fools people to connect to it instead of the real one.