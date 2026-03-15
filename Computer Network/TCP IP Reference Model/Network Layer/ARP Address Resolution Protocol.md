**ARP**: a data link layer protocol that helps map IPv4 to MAC
Features:
	broadcast
	**each devices(hosts, switches and routers)** connected to the network has a **ARP cache table**
	used in a single network not across networks
Process:
1. A broadcasts searching for MAC of with IP of B
2. B unicasts to A about its MAC
## Linux Command
```
# show arp table
arp -n
# manually add to arp table
arp -s <IP> <MAC>
# remove an ip record in the arp table
arp -d <IP>
# clear arp table
ip -s -s neigh flush all

# disable IP forwarding
sysctl net.ipv4.ip_forward=0
```