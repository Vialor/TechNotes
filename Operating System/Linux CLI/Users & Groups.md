## Files
```bash
/etc/passwd 用户ID
	/etc/passwd- backup
# username:password(usually hidden):UID:GID:descriptive info:user file path:login shell path

/etc/group 组ID
	/etc/group- backup
	
/etc/shadow 口令文件
	/etc/shadow- backup
```
## CRUD
```bash
groupadd -g <GID>
groupmod -g <GID> -n <group name>
groupdel <GID>

useradd
passwd <username> # set password
usermod
userdel # -r: also remove user home folder

who/whoami
id
finger

# switch user
su - <username> # do not inherit env vars
su <username> # inherit env vars
```
## Permissions
```bash
sudo CMD # superuser do

chmod <augo><+-><rwx> file_name
a: all; u: user; g: group; o: others
alternatively:
r: 4; w: 2; x: 1
e.g. chmod 755 example_file.txt

chown -R <user> <file>
chgrp -R <group> <file>
```
## Environments
```
/proc/version # os info
/proc/cpuinfo # cpu info
```
Environment variables: variables per user
```
.bashrc: 非登录Shell时运行（本机打开终端）
.bash_profile: 登录Shell时运行（SSH）
```