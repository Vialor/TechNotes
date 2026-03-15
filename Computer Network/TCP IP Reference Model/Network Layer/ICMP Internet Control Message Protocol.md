> used by network devices to diagnose network communication issues
## Format of ICMP Message
Type:  
0 echo reply  
3 destination unreachable  
8 echo  
11 time exceeded  
Code:  
for example, for type 3:  
0 net unreachable  
1 host unreachable  
2 protocol unreachable  
3 port unreachable  
Checksum
## Traceroute & Ping

> **traceroute** works by sending ICMP echo (type 8) messages to the same destination with increasing value of the time-to-live (TTL) field. The routers along the traceroute path return ICMP Time Exceeded (type 11) when the TTL field become zero.

Use TTL from IP: set TTL to 1, A gets back to S; set TTL to 2, B gets back to S; set TTL to 3, finally reached D, D gets back to S.
S → A → B → D

> **PING packet internet groper** in many OS’s by sending ICMP echo (type 8) and receiving ICMP echo reply (type 0)