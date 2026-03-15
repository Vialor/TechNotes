## Tables in Routers
**Forwarding Table**: Mapping from Subnet Number to the interface and MAC of Next Hop
**Routing Table**: Mapping from Subnet Number to Next Hop IP and interface; Built by routing algorithms as a precursor to build the forwarding table
```Python
if SubnetNumber belongs to my interfaces:
	deliver the packet to the interface
elif SubnetNumber in ForwardingTable:
	deliver packet to NextHop router
else:
	deliver packet to default router
```
Route Priority:
longest match: more specific prefix preferred over less specific prefix  
0.0.0.0 matches all prefixes  
## Routing Algorithms
[[Shortest Path Algorithms]]
**route advertisement**: routers sharing routing information with other routers
**route aggregation/summarization**: a method to minimize the number of routing tables in an IP network; it consolidates selected multiple routes into a single route advertisement.
### Distance Vector Routing Algorithm

> A distributed form of Bellman-Ford Algorithm  
> Each router periodically sends its routing table to neighbors  
> Use protocols like RIP Routing Information Protocol  

Evaluation: Simple early approach; it works, but slow convergence after some failures. Link State Routing Algorithm is often used irl.
**Count-to-infinity problem**: Happens when there is a link failure between A and B or A goes offline. With a bad timing, C tells D that 2 hops to reach A before B could tell C about the failure. D then tells B 3 hops to A, and B tells C 4 hops to A. This cycle only ends when the hop number gets really big, and this process is costly.
### Link State Routing Algorithm

> Each router periodically sends to all nodes information about directly connected links  
> Use  
> **IGP (Interior Gateway Protocol)** like IS-IS (Intermediate System to Intermediate System) or OSFP

**Link State Packet LSP (an IP packet)**
IP address of the creator of this LSP
cost to neighbors
SEQNO sequence number
TTL time-to-live
**Reliable Flooding**
store most recent LSP from each node; decrement TTL of each stored LSP, discard when TTL = 0
forward LSP to all nodes except the one sending it
generate new LSP periodically, and increment SEQNO, which starts from 0
**How it works**:
Topology Dissemination: flooding LSP of their portion of the topology (their neighbors)
Route Computation: each node combines all stored LSPs to get the full topology; each node applies forward search algorithm (Dijkstra Algorithm) to compute the routing table