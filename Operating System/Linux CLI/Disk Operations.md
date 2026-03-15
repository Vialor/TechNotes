# Linux磁盘分区
> 格式化磁盘，把数据放在磁盘的不同部位

primary 主分区
extended 扩展分区
logical 逻辑分区
```bash
# 查看磁盘分区， /dev/hda2
fdisk -l

# 创建分区
fdisk <device name>
	fdisk /dev/sda

# 配置文件系统分区
mkfs -t ext3 -b <block bytes> <device name>
```
# 挂载
**挂载/卸载**：将文件系统和目录树结合/分离
	设备文件：/dev/sdb1，磁盘分区后，系统为分区生成的设备文件，其上带有文件系统
		挂载文件系统格式（FAT32）：vfat
	挂载点：/mnt/usb，目录树的一部分
```bash
# 硬件视角：显示系统中的块设备及其挂载信息
lsblk
# FS视角：挂载点的磁盘使用情况
df -h
# 文件视角：看文件/目录本身占了多少空间
du -sh

# 挂载/卸载
mount -t vfat -o ro /dev/sdb1 /mnt/usb
umount /mnt/usb

/etc/fstab # 开机自动挂载的文件系统
```