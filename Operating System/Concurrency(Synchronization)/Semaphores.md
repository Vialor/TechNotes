# Definition in POSIX
**Semaphore** 信号量 is an object with an integer value and three relevant functions:

> Semaphore, when negative, equals the number of waiting threads
```C
// Initilization
sem_t s;
sem_init(&s, 0, X); 
// initialize the integer to be X (X is an int)
// the second parameter 0 means this semaphore is shared among threads of a single process
// Usage
int sem_wait(sem_t *s) {
	decrement the value of semaphore by one
	wait if the value of semaphore is negative
}
int sem_post(sem_t *s) {
	increment the value of semaphore by one
	if there are threads waiting, wake one
}
```
## Binary Semaphores (Locks)
It is binary because a lock is either held or not. Held when non-positive, released when positive(equals 1).
```C
sem_init(&s, 0, 1);
sem_wait(&s)
// critical section
sem_post(&s)
```
Binary semaphores as locks might be an overkill considering the implementation of semaphore.
## Counting Semaphores
**Strong semaphore**: uses a queue (FIFO) of blocked processes. The process unblocked is the one in the queue for the longest time.
**Weak semaphore**: uses a set of blocked processes. The process unblocked is unpredictable.
```C
void *child(void *arg) {
	printf("Step 2\n");
	sem_post(&s);
	return NULL;
}
void main(int argc, char *argv[]) {
	sem_init(&s, 0, 0);
	printf("Step 1\n");
	pthread_t c;
	pthread_create(&c, NULL, child, NULL);
	sem_wait(&s);
	printf("Step 3\n");
	return 0;
}
```
## Semaphores as Condition Variables
Doable but very challenging
# Use semaphores to solve problems
## The Producer/Consumer (Bounded Buffer) Problem
```C
sem_t empty;
sem_t occupied;
sem_init(&empty, 0, MAX);
sem_init(&occupied, 0, 0);
sem_init(&mutex, 0, 1);
void *producer(void *arg) {
	int i;
	for (i = 0; i < loops; i++) {
		sem_wait(&empty);
		sem_wait(&mutex);
		put(i);
		sem_post(&mutex);
		sem_post(&occupied);
	}
}
void *consumer(void *arg) {
	int i;
	for (i = 0; i < loops; i++) {
		sem_wait(&occupied);
		sem_wait(&mutex);
		int tmp = get();
		sem_post(&mutex);
		sem_post(&empty);
	}
}
```
## Reader-Writer Locks
one writer and multiple readers
```C
typedef struct _rwlock_t {
	sem_t lock;
	sem_t writelock;
	int readers;
} rwlock_t;
void rwlock_init(rwlock_t *rw) {
	rw->reader = 0;
	sem_init(&rw->lock, 0, 1);
	sem_init(&rw->writelock, 0, 1);
}
void rwlock_acquire_readlock(rwlock_t *rw) {
	sem_wait(&rw->lock);
	readers++;
	if (readers == 1) {
		sem_wait(&rw->writelock);
	}
	sem_post(&rw->lock);
}
void rwlock_release_readlock(rwlock_t *rw) {
	sem_wait(&rw->lock);
	readers--;
	if (readers == 0) {
		sem_post(&rw->writelock);
	}
	sem_post(&rw->lock);
}
void rwlock_acquire_writelock(rwlock_t *rw) {
	sem_wait(&rw->writelock);
}
void rwlock_release_writelock(rwlock_t *rw) {
	sem_post(&rw->writelock);
}
```
Writing could starve; hence, reading should be throttled
## The Dining Philosophers
```C
void *philosopher(int p) {
	while (1) {
		think();
		get_forks(p);
		eat();
		put_forks(p);
	}
}
void put_forks(int p) {
	put(left(p));
	put(right(p));
}
void get_forks(int p) {
	if (p == 4) { // the last philosopher
		put(right(p));
		put(left(p));
	} else {
		put(left(p));
		put(right(p));
	}
}
```
## Thread Throttling
**throttling:** limit the number of threads concurrently run the code
can be achieved with semaphores