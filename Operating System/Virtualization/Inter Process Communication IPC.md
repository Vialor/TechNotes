# Pipe
```bash
mkfifo myPipe
echo "hello" > myPipe
cat < myPipe
# same as
echo "hello" | cat
```
## `dup2` & `pipe` & `close`
```C
int dup2(int oldfd, int newfd);
File descriptor newfd is adjusted so that it now refers to the same open file description as oldfd
Returns -1 if failed, newfd if success
dup2(fd, 1); // standard input now writes to the open file description of fd

int close(int fd);
Destroy file table entry referenced by element fd of the file descriptor table – As long as no other process is pointing to it!
Set element fd of file descriptor table to NULL

int pipe(int fds[2]);
The pipe system call finds the first two available positions in the process’s open file table and allocates them for the read and write ends of the pipe. This is how two processes can talk to each other.
fd[0] will be the fd(file descriptor) for the read end of pipe.
fd[1] will be the fd for the write end of pipe.
Returns : 0 on Success. -1 on error.
```
# Message Queue
# Shared Memory
# Signal: kill
```bash
# kill a process `ctrl + c`, `kill -9` , `taskkill -f`
kill -l # show all signals
```

`int kill(pid_t pid, int sig);`
send **signals**(pause, die and etc.) to a process
# Socket