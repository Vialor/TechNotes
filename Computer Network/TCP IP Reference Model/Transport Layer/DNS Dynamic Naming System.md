> **DNS**: a service usually provided by ISP to translate domain name to server IP  
> maintained locally in many computers, retrievably gloablly  
   **DNS Name Server**: a server that helps to translate IP addresses into domain names
   **DNS Resolver**: a server responsible for name resolution
# D**omain Name Space**
**Domain Name**: the sequence of labels from a node to the root in the “tree” of _domain name space_ 
**Subdomain:** a domain is a subdomain of another if its domain name ends in the other’s domain name: `support.wix` is a subdomain of `wix`
**Top-level domain**: the direct children of root node `com` `edu` `ca`
**Zone**: a distinct part of the domain namespace which is delegated to a legal entity  
Name servers and zones are multi-to-multi relationship.  
# Domain Resource Records
**Domain resource record**: a five tuple: (Domain_name, Time_to_live, Class, Type, Value)  
Class: usually IN, meaning internet information  
Type: A (IPv4 addr), AAAA(IPv6 addr), CNAME (alias), PTR(unlike CNAME, interpretation depends on context, used in reverse lookups), NS(name server)  
# Name Server Architecture
database server: store authorative records (primary master and slave zone)  
Master – where the data is edited  
Slave – where data is replicated to  
cache: store **cache records** (responses from other name servers)
agent: look up queries on behalf of resolvers
# The Resolution Process
## Iterative
1. Client asks **DNS Resolver**
2. DNS Resolver sends DNS query to **Root Nameserver**  
    Root Nameserver responds with IP of TLD Nameserver  
    **(.edu server)  
      
    **TLD: top level domain
3. DNS Resolver sends DNS query to **TLD Nameserver  
      
    **TLD Nameserver responds with IP of Domain Nameserver **(example.edu server)**
4. DNS Resolver sends DNS query to **Domain Nameserver** repeatedlyDomain Nameserver is authoritative, so reply server IP address
5. Domain Nameserver sends the response to DNS Resolver
## Recursive
1. Client asks DNS Resolver
2. DNS Resolver sends DNS query to Root Nameserver
3. Root Nameserver sends DNS query to TLD Nameserver
4. TLD Nameserver sends DNS query to Domain Nameserver repeatedly
5. The response is sent back recursively
(DNS Recursive Resolver could cache the result)
  
**DNS Hijacking**: DNS Resolver is hijacked and returns false IP