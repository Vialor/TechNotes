**Trusted Computing Base**: everything in a computing system that provides a secure environment for operations, including:  
Most of the hardware except (I/O devices that don’t affect security)  
Part of the kernel (process creation, proc. Switching, mem management, file I/0)  
A **Reference monitor** checks all accesses (system calls for examples) between the trusted and untrusted components.
  
**Protection domain**: a set of (object: files, folders, IO devices, rights: rwx) pairs
**Protection domain** of a process in UNIX is defined by its user id (UID) and group id (GID) (Windows is more granular)
  
Cryptographic Capabilities, to give a process capability, kernel gives out: Token = f(ObjectId + Rights, Secret)
Now when the process wants to access something, it includes the capability, including the token  
The kernel can now compute f(ObjectId+Rights, Secret) and compare it to the Token  
If they match, we know the ObjectId and Rights have not been modified  
  
**Stack Canary**: put a special value in between local variables and the return address so that overflowing a local buffer can be detected.  
Upon entering the function, set a randomly-generated cookie value on the stack and store a backup copy elsewhere. Before executing a ret, check the stack cookie value against the backup and raise an error if it fails
