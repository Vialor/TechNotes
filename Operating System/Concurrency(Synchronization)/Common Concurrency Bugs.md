# Types of bugs
Non-deadlock
Atomicity Violation
Order Violation
Deadlock
# Conditions for deadlock and how to prevent them
## Mutual exclusion

> Exclusive control over resources
Use lock-free approach with potential help from hardware
## No preemption

> Locks can not be forcibly removed from the thread holding them
Give up the first lock if threads cannot acquire the second lock.
Note: small possibility of a **livelock** and implementation difficulty for encapsulation
## Hold-and-wait

> Threads holding locks and waiting for locks

Acquire the all locks at once as an atomic operation using an another lock for example.
Note: We need to know which locks to be held and acquire them ahead of time instead of when we really need those resources. As such, this method decreases concurrency and is against encapsulation.
## Circular wait

> Existence of circular chain of threads holding resources requested by the next thread in the chain

**Total ordering & Partial ordering**: specify the order that locks should be acquired
# Deadlock Avoidance via scheduling
**Dijkstra’s Banker’s Algorithm**: 当一个进程申请使用资源的时候，银行家算法通过先试探分配给该进程资源，然后通过安全性算法判断分配后的系统是否处于安全状态，若不安全则试探分配作废，让该进程继续等待。
safe sequence 安全序列: a sequence if system allocate resources in this manner, there will be no deadlocks and every thread can finish
不安全状态: 系统找不到任何安全序列（possible for deadlocks (provided no resource is returned)）
# Detect and Recover from Deadlocks
**Use a graph for resource requests and allocation**
Thread Node, Resource Node  
Thread Edge: How many resources it has requested, one edge for one resource  
Resource Edge: How many resources it has given away, one edge for one resource  

**Detect deadlocks**
Find safe sequence regarding the graph (Dijkstra’s Banker’s Algorithm for example)

**Recover from deadlocks**
资源剥夺 (be careful to not to starve the thread depraved of the resources)  
终止进程 (expensive)  
进程回退 (go back to a point where deadlocks can be avoided)