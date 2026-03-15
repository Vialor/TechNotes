the ability of different parts of a program running out of order or in partial order
Motives?
to achieve **Parallelization**: Transforming a single-threaded program into a program that runs on multiple CPUs.
# Threads
> a portion of the **process**

The underlying difference from a process is memory usage:
1. Threads of the same process share the same address space
2. A multi-threaded process has independent stacks for each thread at the top of the address space.
Process is a concept when we use a single CPU to handle multiple tasks. Threads is a concept when we use multiple CPUs to handle a single task.
**Thread Control Block TCB**
## Threads in Unix
```Python
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void*), void *arg);
int pthread_join(pthread_t thread, void **value_ptr);
void pthread_exit(void *value_ptr);
```
# Basic Terms
**Race Condition:** The results depend on the timing execution of the code
**Atomic Operations**: undividable operations; operations that are so low-level that it cannot be interrupted
**Synchronization Primitives**: atomic operations supported by OS to achieve concurrency for multi-threaded code
**Transaction**: the grouping of many actions into an atomic operation
**Critical Section:** A segment of code reading or modifying information that is shared with other threads. A critical section must not be executed by multiple threads **concurrently**.
# Achieve Concurrency
1. read and modify shared resources: by supporting atomicity for critical sections with **locks**
2. tell thread A to wait for thread B: by supporting sleep/wake interactions with **condition variables**

> **Locks**: methods of synchronization used to prevent multiple threads from accessing a resource at the same time  
> **Condition variables**:  synchronization primitives that enable threads to wait until a particular condition occurs

**Locks**

|                 | Optimistic Lock (Lockless)                                                                      | Pessimistic Lock                              |
| --------------- | ----------------------------------------------------------------------------------------------- | --------------------------------------------- |
| Use Case        | read a lot and write little                                                                     | write a lot and read little                   |
| Mechanism       | change shared resources and then check for conflicts; if conflict, drop the operation and retry | lock before change shared resources           |
| Implementations | How to know conflict: version number, CAS                                                       | [[Spin Lock & Mutex]]<br>[[Read Write Locks]] |
| Examples        | Git, shared online docs                                                                         | DB row-level lock and table-level lock        |

**Semaphores:** a general solution to both locks and condition variables
[[Semaphores]]

[[Common Concurrency Bugs]]