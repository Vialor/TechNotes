```bash
# static snapshot
ps aux # process status
# a = show processes for all users  
# u = display the process's user/owner  
# x = also show processes not attached to a terminal

# dynamic and interactive monitoring
top

# show processes in an ACSII tree
pstree -up

# send signal
kill pid # requests a process to terminate but allows it to perform cleanup operations before terminating.
killall <process name> # kill all processes in a group
kill -9 pid # forces a process to terminate immediately.
kill -19 pid # SIGSTOP
kill -18 pid # SIGCONT

# jobs
jobs # show jobs attached to current session
nohup # job will not be suspended (run after current shell is closed)
nohup sh -c 'sleep 30; touch a' &
bg %jobID # put job in background
fg %jobID # put job in foreground

# future jobs
crontab -u <username> <-e|-l|-r> # set cron job
at 19:26 # run command at 19:26
at -l

# list file processes
lsof -p <PID> # list files opened by a process
lsof /path/to/file # list processes using this file
lsof -i # check network connection
```
# Service Control
systemd: system daemon
```bash
systemctl start | stop | status <service>

# Journal: /etc/systemd/journald.conf
journalctl -u A.service
```