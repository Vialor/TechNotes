Capture filter: filter during capture, filter based on Network layer and below
Display filter: filter after capture
# Display Filter
```Wireshark
ip.addr == 172.21.2.116
ip.src
ip.dst

tcp
tcp.port
tcp.stream
http
http.request
http.response
http.host

// slicing
eth.src[start offset : length] == 32.65.76
eth.src[start offset - end offset]
// membership
tcp.srcport in {80 8080 80..8080}
// comparator
contains <string>
matches <Perl-compatible regular expressions PCRE>
```
# Capture Filter: BPF Syntax (Berkeley Packet Filter)
`tcpdump -r [read_from_file.pcapng] -w [write_to_file] [BPF_SYNTAX]`

```
Type:
host, net, port, portrange
host iml
port 20

Dir (Direction):
src, dst
src iml
dst net 138.4

Proto (Protocol):
wlan, ether, ip, ip6, tcp, arp
tcp port 20
arp net 128.5
```