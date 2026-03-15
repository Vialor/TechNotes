# Process
The file descriptor table is part of the individual process's memory, mapping file descriptors (which is basically the indices) to pointers to file table entries.
## `fork` & `exec` & `wait`
```C
int pid = fork()
if (pid == 0) {
   // child
} else if (pid > 0) {
	// pid is the pid of child
	// parent
} else {
	// error
}
int execl(const char *path, const char *arg, ...);
int execlp(const char *file, const char *arg, ...);
int execle(const char *path, const char *arg, ..., char * const envp[]);
int execv(const char *path, char *const argv[]);
int execvp(const char *file, char *const argv[]);
int execve(const char *path, char *const argv[], char *const envp[]);
// exec does not create a new process; it just changes the program file that an existing process is running
```
## Others
parents are entitled to receive the  **exit codes** of its children
`getpid()`, `waitpid()`, `killall()`
# Threads
```C
pthread_create(&threads[i], NULL, apply_read, arg);
pthread_join(threads[i], NULL);
pthread_exit(NULL);
```