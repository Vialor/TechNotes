# Overview
Memory alone is not be enough to hold all the valid pages. We need help from, for example, disks.
**Swap Space**: Space on disk for moving pages back and forth. It is a part of physical memory.  
Note: It is not the only space to swap with memory, files on disk themselves for example could swap with memory  
**Present Bit**: a bit on PTE to show whether the page is present in memory
## Page into memory
**Page Fault**: When present bit is false, whoever manages TLB misses raises a page fault, and asks **page fault handler** (part of OS) to handle it. Page fault handler is responsible for fetching the needed pages from disk to memory. (handler will need issue an disk I/O, and thus temporarily put the process on a blocked satte)
Q: How does page fault handler know where the pages are on disk?
A: We store the disk locations on the should-be-PFN bits in PTEs. Handler would just need to check the page table.
## Page out of memory
**swap daemon/page daemon**: OS runs **swap daemon** when free pages are needed, and reawaken the original thread.
Avoid using up all the memory: When OS notice there are fewer free pages than **Low Watermark**, it awakes swap daemon to start evicting pages from memory till free pages of **High Watermark**.
# Policies
## Replacement Policy
The Optimal Replacement Policy (but unpractical): evict pages that will be accessed furthest in the future
- **Types of Cache Misses**
    
    compulsory miss/cold-start miss
    
    conflict miss
    
    capacity miss
    
**FIFO First In First Out, Random**
**LFU Least-Frequently-Used,** **LRU Least-Recently-Used**
  
**Belady’s Anomaly**: cache hit rate worsens when increasing cache size  
  
**Stack Property**: cache of N+1 size contains the content of cache of N size  
FIFO and Random don’t have stack property  
### Evaluation
Comparing Hit Rate and Cache Size with different types of workloads:
No-Locality Workload: LRU = FIFO = Random
80-20 Locality Workload (80% queries are made to 20% pages): LRU > FIFO = Random
Looping Workload: have corner cases for LRU and FIFO, Random does better
### Implement LRU by Approximation
**use bit/reference bit**: a bit that will be set to 1 by hardware when the corresponding page is referenced, OS will decide when the bit is set to 0.
**clock algorithm**: A clock hand scans through pages. If the use bit of a page is 1, set it to zero and skip it; if the use bit of a page is 0, evict it.
**dirty bit/modified bit**: a bit for a page, it is set to 1 when the page is written. Evicting unwritten pages first is preferred to avoid writing back to the disk which is expensive.
## Other VM Policies
**Page Selection Policies**: when to load pages into memory  
  
**demand paging, prefetching**
Policy for when to write to the disk: **clustering/grouping**
# Thrashing

> The system is spending a significant amount of time and resources swapping data between the main memory and the storage.  
> Usual cause: Memory demanded by running applications exceeds physical memory  
**admission control**: decide not to run a subset of current processes