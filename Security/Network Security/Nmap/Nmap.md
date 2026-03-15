```python
# Host Discovery
nmap -sn 192.168.1.0/24

# Port Discovery
nmap
	-p <port ranges> <IP/domain name> # scan specified ports (by default 1-1000)
	-p- <IP/domain name> # scan all ports
	
	-sS # scan TCP ports: SYN scan (default)
	-sT # scan TCP ports: TCP Connect scan (default fallback)
	-sU # scan UDP ports
```
# Ncat
``` bash
ncat -l -p 1267 < test.txt
ncat 10.1.1.198 1267 > test.out
```