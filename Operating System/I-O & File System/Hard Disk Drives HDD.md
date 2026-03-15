# A**natomy**
## Disk Drives
spindle
disk head & disk arm
platters
surfaces(2 per platter)
tracks
sectors(512-byte blocks)
### Details
Sector write is guaranteed to be atomic. Incomplete write due to untimely halt is a **torn write**.
## I/O Operations
seek: acceleration, coasting, deceleration, settling
rotation (rotational delay)
transfer: actual reading/writing data
### Details
**Track skew**: skew neighboring tracks for a few blocks to facilitate sequential read across tracks. It avoids disk head misses next sector on the next track and thereby reduces the rotational delay.
**交替编号**：让逻辑上相邻的扇区在物理上有一定间隔从而避免transfer时错过sector。
**Track buffer/Cache**: disk might decide to read all sectors on the current track in advance.
# Disk Scheduling

> seek time and rotational delay are usually close, and transfer time is relatively small comparing to them

### FCFS First-Come, First-Served
### SSTF Shortest Seek Time First
good (not the best comparing to SATF) turn around time, but could lead to starvation
### Elevator Algorithm(SCAN, C-SCAN)
**sweep**: disk head move from outer track to inner track
SCAN sweeps the disk back and forth for seeking. C-SCAN (Circular Scan) only sweeps the disk unidirectionally to be avoid favoring middle tracks. F-SCAN (Freeze Scan) freeze the I/O request queue while sweeping.
### Modern Solution (consider rotational delay)
**SATF Shortest Access Time First**
SATF is performed in disk drives, not by OS since position is unknown to OS
I/O Merging  
  
**OS merges requests to neighboring blocks into a single request.**
**Work-conserving**: OS issues I/O request to HDD immediately  
**Non-work-conserving**: OS waits for a mysterious time before sending all the I/O requests to HDD