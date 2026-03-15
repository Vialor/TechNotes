# Implement a lock
```C
typedef struct __lock_t {
	int flag;
} lock_t;
void init(lock_t *lock) {
	lock->flag = 0;
}
void unlock(lock_t *lock) {
	lock->flag = 0;
}
void lock(lock_t *lock) {
	// TODO
}
```
## Hardware Primitives (atomic operations to build a lock)
### test-and-set(atomic exchange)
```C
int TestAndSet(int *old_ptr, int new) {
	int old = *old_ptr;
	*old_ptr = new;
	return old;
}
void lock(lock_t *lock) {
	while (TestAndSet(&lock->flag, 1) == 1)
		; //spin
}
```
### ==CAS compare-and-swap==
> More powerful than test-and-set in **lock-free synchronization**
```C
int CompareAndSwap(int *ptr, int expected, int new) {
	int original = *ptr;
	if (original == expected)
		*ptr = new;
	return original;
}
void lock(lock_t *lock) {
	while (CompareAndSwap(&lock->flag, 0, 1) == 1)
		; //spin
} 
```
### load-linked & store-conditional
```C
int LoadLinked(int *ptr) {
	return *ptr;
}
int StoreConditional(int *ptr, int value) {
	if (no update to *ptr since LoadLinked to this address) {
		*ptr = value;
		return 1;
	} else {
		return 0;	
	}
}
void lock(lock_t *lock) {
	while (LoadLinked(&lock->flag) ||
		!StoreConditional(&lock->flag, 1))
		; // spin
}
```
### fetch-and-add
```Python
int FetchAndAdd(int *ptr) {
	int old = *ptr;
	*ptr = old + 1
	return old
}
```
**Ticket lock** (a guarantee that this thread will run in the future. Unlike previous locks where the thread could be spinning forever waiting for other threads)
```Python
void lock(lock_t *lock) {
	int myturn = FetchAndAdd(&lock->flag)
	while (lock->turn != myturn)
		; // spin
}
```
# Instead of Spinning
Spinning is bad because when thread A is interrupted in a critical section, CPU would stuck with thread B which is only spinning and wasting the entire time slice.
Spinning could also lead to **Priority Inversion (a bug where high priority tasks are preempted by low priority ones):** Low priority tasks holding the flag prevent high priority tasks running, and high priority tasks prevent low priority tasks running because of their priority, so the system is a hung, which is a serious **correctness** issue.
One general solution to **Priority Inversion** is **Priority Inheritance:** temporarily ask the low priority tasks in critical section to inherit the priority of the high priority tasks.
## Yielding to CPU
B makes a system call to transfer the thread from running to ready state.
It is better than spinning, but context switching is still costly especially when there are many B’s waiting for A.
## Queue and Sleeping: Solaris solution
Help from the OS:
`park()` put current thread to sleep
`unpark(threadID)` wake up the thread with `threadID`
```C
typedef struct __lock_t {
	int flag;
	int guard;
	queue_t *q;
} lock_t;
void init(lock_t *m) {
	m->flag = 0;
	m->guard= 0;
	queue_init(m->q);
}
void lock(lock_t *m) {
	while (TestAndSet(m->guard, 1) == 1)
		; // acquire guard lock by spinning
	if (m->flag == 0) {
		m->flag = 1;
		m->guard = 0;
	} else {
		queue_add(m->q, gettid());
		m->guard = 0;
		park();
		// do not set flag to 0 here because th thread is not holding guard
	}
}
void unlock(lock_t *m) {
	while (TestAndSet(m->guard, 1) == 1)
		; // acquire guard lock by spinning
	if (queue_empty(m=>q)) // flag only releases when the queue is empty
		m->flag = 0;
	else
		unpark(queue_remove(m->q));
	m->guard = 0;
}
```
`guard` a spin lock that protects flag and queue manipulation
**Wakeup/Waiting Race**: Suppose thread A is holding the lock, then B tries to acquire the lock. Just before thread B executes `park();` , the scheduler switches to thread A and A unlocks. A will unpark B before B parks! This could lead to B sleeping forever.
Solution from Solaris: use `setPark();` instead of `park();` .
### Linux Solution:
1. **futex:**
Help from the OS:
`futex_wait(address, expected)` if address == expected: sleep; else: return
`futex_wake(address)` wakes a waiting thread in the queue
2. **two-phase lock:** spin lock → mutex lock
    mutex: instead of spinning, yield immediately