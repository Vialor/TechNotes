## Attacks
**IP Spoofing**: fake IP to look legit
Difficulties:
	TCP handshake cannot be completed because server SYN response will be lost since IP in the SYN request is fake
	BGP broadcast will tell upstream network which IP prefixes are legit
- - -
**Session Hijacking**: In a TCP conversation, the attackers sniff the exchanging packets and jump in and spoofing themselves as the client
Difficulties: The server gets different seg #'s from the attacker and client, and it will know there is a problem
	Solution: **ARP spoofing**: send unsolicited ARP replies to the server and the client to overwrite their IP-to-MAC ARP tables, so they send packets to no where (or the attacker's MAC) instead of each other, and the attacker can continue spoofing as the client
- - -
**DoS Denial of Service**: prevent legitimate user accesses or stop critical system processes
	Connection flooding: fill the connection queue with SYN flood
	Bandwidth flooding attack: overwhelm communication link with packets
	Vulnerability attack: Maliciously crafted messages that use up the server's resources, and thus remotely stop or crash services
**DDoS Distributed DoS**:
	1. **Bot network**: The attacker takes over many machines as bots to flood a target
	2. **Reflection attack**: The attacker send requests to multiple third party servers (DNS servers for example) which has the the source IP as the IP of victim. Third party servers' replies will then flood the target.
		**Amplification attack**: manage to make third party servers replies much larger replies to the victim even though the attacker's initial request is small
			**Smurf attack**: attacker sends ICMP echo with victim's IP to a network's broadcast address.

Connection flooding defense: **SYN cookies** (Put the session info into cookies, not the server)
Server calcs hash based on IPs and ports of 2 parties and a secret number, then uses the resulting cookie for its initial seq # (ISN) in SYNACK. The key is that the server does not allocate anything to half-open connection. When a legitimate client returns ACK, the server computes the same hash and verifies that ACK # = ISN + 1 before creating socket for the connection.
- - -
**DNS attack**:
	Redirection: Attacker responds with a bogus DNS reply like it is from a DNS server (Attacker also needs to mute the response from the true DNS server, which is difficult)
	Poisoning local DNS server: Attacker tricks the local DNS server into caching the wrong IP address by pretending to be higher level DNS server and answering the requests from the local DNS server
## IPv6
**Privacy Extensions**: Routers generate temporary random IPv6 address +interface ID for each network interface
**uRPF Unicast Reverse Path Forwarding**: Routers prevent L3 spoofing by checking if how the packet arrives match the reverse path stored in routers.
## IPSec
> Used in unknown or even hostile environment

IPSec Implementations:
	**AH Authentication Header**: Compare a hash value from a secret key shared by both sides
	**ESP Encapsulating Security Protocol**: encrypt IP header (in tunnel mode) and payload
	AH focuses on authentication; ESP provides both authentication and confidentiality

**PKF Perfect Key Forwarding**: If a key is compromised, only the specific session it protects will be revealed to the attackers. Past or future sessions are not affected. To accomplish this, each session has its own key, which is established before the session and deleted after. 
### IKE Internet Key Exchange
**IKE** is a protocol that uses **ISAKMP Internet Security Association and Key Management Protocol** (define a framework to protect later SA negotiation), and **Oakley Key Determination Protocol** (a refinement of Diffie-Hellman for key exchange).

**Transform set**: a static setting on devices
	Encryption Algorithm: AES & 3DES
	Authentication Algorithm: HMAC-SHA256
	Mode: Transport mode & Tunnel mode (VPN is usually on IPSec tunnel mode)
**SA Security Association**: a dynamic setting established through negotiation, which considers relevant transform sets.
	SPI (Security Parameters Index): an ID for SA
	Encryption Algorithm & Authentication Algorithm
	Symmetric key used for encryption and authentication
	Traffic Selector: defines the scope of traffic protected by the SA (e.g., source/destination IP addresses, ports, protocols)
	Key Lifetime
### 5 Steps of IPSec Operation:
1. Interesting traffic: VPN devices recognize the traffic to protect
2. IKE phase 1: Negotiate an **ISAKMP SA** to protect later SA negotiation (a.k.a establish a secure channel)
3. IKE phase 2: Negotiate an **IPSec SA** to protect the actual data
4. Data transfer: VPN devices apply security services to traffic and then transmit data
5. Tunnel terminated

