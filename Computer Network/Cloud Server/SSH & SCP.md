**SSH (Secure Shell)** is a protocol based on *Asymmetric Encryption*
**SCP (Secure Copy Protocol)** is a protocol based on SSH  
**OpenSSH** and **PuTTY** are software implementation of SSH  
**WinSCP (Windows Secure Copy)** is a Windows software that provides secure file transfer using SSH), SFTP(SSH FTP) and SCP

```Shell
# test connection
ssh username@hostwebsite
# use key pair to login
ssh -i path/to/private/key username@hostwebsite
# manually input password to login (less secure)
ssh -i username@hostwebsite
scp local/file/path username@hostwebsite:remote/file/path
```
## SSHD
sshd =  SSH daemon
	这个守护进程通常由OpenSSH实现提供，并且通常监听在22端口上
	服务器需要sshd才能被ssh链接

安装配置
```
dnf install -y openssh-server
ssh-keygen -A # 生成key
passwd root # 设置root用户的密码
```
配置sshd_config
```
vi /etc/ssh/sshd_config

Port 2222              # ⚠️ 强烈建议不用 22
ListenAddress 0.0.0.0
PermitRootLogin yes    # 调试用，生产慎重
PasswordAuthentication yes
UsePAM no
AllowTcpForwarding yes
```
启动
```
/usr/sbin/sshd -D # 调试
/usr/sbin/sshd #后台运行
```
## Mechanism
### Keys
For client to SSH into server, a pair of private and public key of client is needed, private key is stored in client machine.
Server use private and public key to decide if client can access.
### Ports
Server local port for ssh: usually 22
Server exposed port for ssh: for example 37298
## Files
### ~/.ssh
authorized_keys: authorize clients (users) to access the server 
	encrypt algorithm, public key, comment 
known_hosts: verify the identity of the server to the client
	server domain, IP, encrypt algorithm, server public key
<key_name>: client private key (also .pem for OpenSSH, .ppk for PuTTY)  
<key_name>.pub: client public key  
```Shell
# add public key to authorized_keys of the server
ssh-copy-id -i ~/.ssh/<public _key>.pub username@hostwebsite
# create key pair 
ssh-keygen -t <algorithm> -C <comment>
## algorithm: rsa
# generate public key from private key
ssh-keygen -y -f ./<private_key>.pem
```
config:
```Plain
Host <name>
	HostName <host domain>
	IdentityFile ~/.ssh/private_key.pem
	User username
	Port 42337
    LocalForward 8080 localhost:8080 # forward server local host to client
# so we can use `ssh <name>` to login in the future
```
### /etc/ssh
sshd_config:
```Shell
# in server machines use the following to force key login
PubkeyAuthentication yes
PasswordAuthentication no
```
