# Scheduling Metrics
**General Goals:**
- **Performance**: speed to finish tasks
    (Average) **Turn Around Time** := Completion Time - Arrival Time
    **Throughput** := jobs completed / time interval
- **Fairness**: be fair to all tasks and have a priority
    **Jain’s Fairness Index**
- **Interactivity**: respond to user input quickly
    (Average) **Response Time** := First Run Time - Arrival Time
**Specific Goals:**
Batch system(one job at a time): Performance, Fairness
Interactive System(jobs are chopped into small pieces): Interactivity
Real-time system: Meeting deadlines(avoid losing data), Predictability(avoid quality degradation)
# **Scheduling Principles**
## Batch System
==Simple and lazy solution:==
**FIFO First In First Out**
**Convoy Effect:** heavy task keeps other tasks waiting and thus results in a low performance

==Good for turn around time, bad for response time:==
**SJF Shortest Job First**
Note: The optimality of turn around time only exists when all jobs are available at time 0.
**STCF Shortest Time-to-Completion First / PSJF Preemptive Shortest Job First**

==SJF with a compromise for response time:==
**HRRN Highest Response Ratio Next**
Response Ratio = (Waiting Time + Burst Time) / Burst Time
## Interactive System
**Preemption**: the act of temporarily interrupting an executing task, with the intention of resuming it at a later time. (**preemptive/non-preemptive schedulers)**
Time is divided into **quanta/quantum/time slice** to run a process before returning back to the scheduler

==Good for response time, bad for turn around time:==
**RR Round Robin / Time Slicing**
good for response time, bad for turn around time
To incorporate I/O, each CPU burst is treated as a separate job
  
Guaranteed Scheduling (Proportional-share scheduler, Fair-share scheduler)
**Lottery Scheduling**: we give each process a fixed number of lottery tickets
ticket currency
ticket transfer: a process can temporarily hand off its tickets to another process. This ability is  
especially useful in a client/server setting, where a client process sends  
a message to a server asking it to do some work on the client’s behalf.  
To speed up the work, the client can pass the tickets to the server and  
thus try to maximize the performance of the server while the server is  
handling the client’s request. When finished, the server then transfers the  
tickets back to the client and all is as before  
ticket inflation
### MLFQ Multi-level Feedback Queue
A trade-off between turn around time and response time
**Basics:**
**rule1: If priority(A) > priority(B), A runs, B waits**
**rule2: If priority(A) = priority(B), A, B run in RR**
  
**Problem:**
How the priority is changed overtime? Or why some tasks should have higher priority than others?
**Solution:**
**rule3: New jobs are placed at the highest priority**
rule4a: If a job uses up an entire time slice, reduce its priority by 1
rule4b: If a job gives up the CPU before the time slice is up, it stays at the same priority level
(We don’t punish a job for its interactivity)
  
**Problem:**
The Starvation of long-running and CPU-bound jobs
CPU-bound jobs may changed to a phase of interactivity
**Solution:**
**rule5: After time period S, move all jobs in the system to the topmost queue**
S is an example of **voo-doo constants**: a somewhat hacky constant to be fine-tuned to work magically
  
**Problem:**
Gaming the scheduler: a program always issues an unnecessary I/O operation just before the time slice to gain unfair amount of CPU resources
**Solution:**
Rewrite **rule4: Once a job uses up its time allotment of a time slice regardless of how many times it gives up CPU, reduce its priority by 1**
  
**Tweak the voo-doo constants**
> Two voo-doo constants here: time slice and time period S

Use shorter time slice for higher priority queue: short-running interactive jobs
Use longer time slice for lower priority queue: long-running CPU-intensive jobs
Learn from user **advice** `nice`
## Real Time System
Multimedia systems & Control systems
Events in real time system can be **periodic** or **aperiodic**
The system has to be **schedulable**: for each periodic event, P: event period, C: process time, $\sum^m_{i=1} {C_i \over P_i} \leq 1$