# Files
## Properties
name, path, identifier, external storage location
type(extension), size
ownership, permission
date created, date modified
## Contents
**Unstructured data** & **Structured data**
> directory is a special file of structured data

**File descriptor**: a property of a process handling reading and writing
all access to the file data is done by asking OS for the file descriptor
shell always ensures that 3 file descriptors open: `stdin 0, stdout 1, stderr 2`
# VSFS: Data Structure
## Overall Organization
disk is roughly divided into following **block** areas:
	superblock: particular info about the FS itself (like how many inodes and data blocks)
	inode table: inodes
	allocation structure: keep tracks of the status of blocks, for example an inode bitmap (for inode table) and a data bitmap (for data region)
	data region: data blocks
## File Organization
i-number → inode → data blocks
**inode**: index node (historical reason): a general term describing the structure that holds properties of a given file.
**i-number**: a low level name for inode, OS should know how to find out which sector the inode is in based on i-number.
### From inode to data blocks (external storage location issue)
**Multilevel Index (pointer based)**
**indirect block**: a block containing indirect pointers
**indirect pointer**: a pointer that points to a data block that contains more pointers, each pointing to a direct block
also: double indirect pointer, triple indirect pointer
For example, a file data would use 12 direct pointers and then add a indirect block and a double indirect block procedurally when the file gets larger. This can handle `(12 + 1024 + 1024^2) * block_size` data.
It is based on the assumption that most files are small
  
**Extent**
**extent**: a disk pointer + a length in blocks
might have a hard time finding contiguous free space: FS allows for more than one extent to distribute the file data.
less flexible than pointer method, but fewer metadata and thus more compact
  
**Linked list**
(used in FAT)
data blocks are designed to be a linked list
bad for random access
## Directory Organization
> directory is a special file of structured data

It contains file names (including . and ..) and i-numbers.
# File Link
在 Linux/类 Unix 系统上，文件链接（File Link）是一种特殊的文件类型，可以在文件系统中指向另一个文件。常见的文件链接类型有两种：
**1、硬链接（Hard Link）**
- 在 Linux/类 Unix 文件系统中，每个文件和目录都有一个唯一的索引节点（inode）号，用来标识该文件或目录。硬链接通过 inode 节点号建立连接，硬链接和源文件的 inode 节点号相同，两者对文件系统来说是完全平等的（可以看作是互为硬链接，源头是同一份文件），删除其中任何一个对另外一个没有影响，可以通过给文件设置硬链接文件来防止重要文件被误删。
- 只有删除了源文件和所有对应的硬链接文件，该文件才会被真正删除。
- 硬链接具有一些限制，不能对目录以及不存在的文件创建硬链接，并且，硬链接也不能跨越文件系统。
- `ln` 命令用于创建硬链接。
**2、软链接（Symbolic Link 或 Symlink）**
- 软链接和源文件的 inode 节点号不同，而是指向一个文件路径。
- 源文件删除后，软链接依然存在，但是指向的是一个无效的文件路径。
- 软连接类似于 Windows 系统中的快捷方式。
- 不同于硬链接，可以对目录或者不存在的文件创建软链接，并且，软链接可以跨越文件系统。
- `ln -s` 命令用于创建软链接。