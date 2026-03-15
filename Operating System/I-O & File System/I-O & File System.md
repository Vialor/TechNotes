**Device Controllers**: a hardware that understands "software" input. It translates software input into something a hardware device understands. Controllers sometimes have their own memory and their own CPU.  
The device controllers also run software and have their own operating systems, which is  
**Firmwares**.
**Device Drivers**: purely software. Device drivers are loaded as kernel modules to communicate between the kernel and physical device controller.
# I/O
[[IO Models]]
[[IO Devices]]
# File System
**传统型文件系统**：先写数据，有空慢慢补元数据，不安全
**日志执行文件系统**：同时写数据和元数据，断电可恢复内容

**非索引式文件系统**：读取数据需要一个一个block以linked list形式去进行
**索引式文件系统**：文件属性数据放在inode和实际数据放在block，根据inode可快速找到blocks
	读目录树的过程：/etc/group: superblock --> 根目录i-number --> 找到根目录inode --> 读取根目录block -->找到etc文件夹inode --> 读取etc block --> ......

[[Very Simple File System VSFS]]
