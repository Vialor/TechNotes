- A routing protocol used to exchange routing information between different ASes. The true purpose of BGP is controlling how traffic enters the local AS, rather than how traffic exits it
- Runs over TCP port 179.
- BGP maintains a _separate_ routing table based on shortest AS Path and various other attributes, as opposed to IGP metrics like distance or cost.
# BGP Terminology
**AS Autonomous System**: Group of IP based networks with the _same routing policy_.
**ASN Autonomous System Number**: Globally unique identifier for IP networks  
It is a 16-bit number ranging from 1~65535.  
**AS path**: a collection of numbers of ASes that pass throught the source AS to destination AS
  
**Neighbors/Peers**: Any two routers formed a TCP connection exchanging BGP routing information  
(No need to be directly connected, such connectivity is handled by  
**IGP Interior Gateway Protocol** like OSPF, RIP and IS-IS)
**iBGP internal BGP**: protocols for BGP neighbor relationship within the same AS: exchange routing info from outside of AS (unlike IGP, which exchanges routing info from inside of AS)  
All iBGP speakers should peer with all other iBGP speakers in the AS  
**eBGP external BGP**: protocols for BGP neighbor relationship between two peers from different AS  
(By default, should be directly connected)  
  
An AS is a … when …:  
	**Transit**: carrying traffic across a network, usually for a fee  
	**Peer**: exchanging routing information and traffic  
	**Default**: is the place to send traffic when there is no explicit match in the routing table
# BGP General Operation
learns multiple paths via internal/external BGP speakers
pick the best path and installs it in the routing table (RIB Routing Information Block), then the forwarding table.
send the best path to external BGP neighbors
apply policies by influencing the best path selection