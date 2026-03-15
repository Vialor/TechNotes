> A tool for centralized management of TCP/IP address within LAN with no knowledge of IP; it provides configuration parameters to the hosts including IP address, address mask, router address, host name and etc.  
> An extension of BOOTP, uses UDP datagram  
> RFC2131 & RFC2132  
## Types of address allocation
automatic allocation: address is permanently assigned
dynamic allocation: address is temporarily leased
manual allocation: address is assigned manually by admin
# DHCP Header
## Opcodes
**Typical steps:**
Discover: client _broadcasts_ to locate an available server
Offer: server _unicasts_ to client with an offer of configuration params
Request: client _broadcasts_ to server requesting offered params from the server
ACK: server _unicasts_ to client with config params, including IP address
(NAK: server sends to client refusing requests for config params; client has to start over)

> A client that cannot receive unicast IP datagrams until its protocol software has been configured with an IP address SHOULD set the BROADCAST bit in the 'flags' field to 1 in any DHCPDISCOVER or DHCPREQUEST messages that client sends.  
> In this case, server broadcasts to client in these steps  

**Other messages:**
Release: client sends to server to release assigned address
Decline: client sends to server indicating config params invalid
Inform: client sends to server for more network configs typically when IP has been statically assigned
## Transaction ID xid
a random number chosen by clients
it associates the messages and responses between a client and a server