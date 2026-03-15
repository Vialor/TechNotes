# Socket in Python
server.py
```Python
from socket import *
IP = '127.0.0.1'
# 0.0.0.0: all ip addresses of this machine
# 127.0.0.1: loop back address 
PORT = 50000
MAX_QUEUED_CONNECTIONS = 5
BUFLEN = 1024
listenSocket = socket(AF_INET, SOCK_STREAM) # AF_INET: ipv4, SOCK_STREAM: tcp
listenSocket.bind((IP, PORT))
listenSocket.listen(MAX_QUEUED_CONNECTIONS)
while True:
	dataSocket, addr = listenSocket.accept() # Three-way handshake
	while True:
		data = dataSocket.recv(BUFLEN)
		if not data: # received empty bytes
			break
		info = data.decode() # decode into strings
		dataSocket.send(f'server has received {info}'.encode())
dataSocket.close()
listenSocket.close()
```
client.py
```Python
from socket import *
SERVER_IP = 'xx.xx.xx.xx'
SERVER_PORT = 50000
BUFLEN = 1024
dataSocket = socket(AF_INET, SOCK_STREAM)
dataSocket.connect((SERVER_IP, SERVER_PORT)) # Three-way handshake
while True:
	# read user input from terminal
	toSend = input('>>> ')
	if toSend == 'exit':
		break
	dataSocket.send(toSend.encode())
	data = dataSocket.recv(BUFLEN)
	if not recved:
		break
	info = data.decode() # decode into strings
dataSocket.close()
```
  
SOCK_DGRAM: udp
```Python
# connectionless and thus no need for accept() and connect().
sendto(data, (target_ip, target_port))
dataSocket.recvfrom(BUFLEN) # returns (data, addr)
```
![[Untitled 9.png|Untitled 9.png]]
**Socket**: endpoint of a two-way communication link between two programs running on the network