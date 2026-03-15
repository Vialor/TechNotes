**Schedule**: the order to execute actions in multiple transactions
# ACID
**Atomicity**: the system either execute all operations in a transaction or none of them
	**Undo logs**: Rollback any change if a transaction failed
**Consistency**: through out the transaction, the system adheres to all database constraints
**Isolation**: no race condition among transactions 
	**MVCC Multi-Version Concurrency Control**
	**Locks**: [[Read Write Locks]]
**Durability**: once a transaction is done, the change should persists despite software or hardware failure
	**Redo Logs**: [[Crash Recovery]]

> On the schedule level, Recoverability is *necessary* for Atomicity & Durability, and Serializability is *sufficient* for Consistency & Isolation.
## Serializability & Recoverability
### Serializability: Consistency & Isolation
A schedule is **serializable** if it is equivalence to *some* serial execution of the transaction
*Conflict Serializability* is a subset of *View Serializability*, which is a subset of *General Serializability*, which is a subset of *correctness*.

A schedule is **conflict serializable** if it is **conflict equivalent** to a serial schedule
	**conflict**: a resource is used by two instructions at the same time and at least one of the instructions is writing. 
	**conflict equivalent**: interchangeable by a series of swaps of non-conflicting instructions
	Testing conflict serializability: **Precedence Graph** has no cycle
		For these cases we add an edge T1 -> T2:
			T1 reads A, then T2 writes A
			T1 writes A, then T2 reads A
			T1 writes A, then T2 writes A

A schedule is **view serializable** if it is **view equivalent** to a serial schedule
	S and S' are **view equivalent** if for each data item Q, they satisfy:
		1. If in S Ti reads the initial value of Q, then in S’ Ti still reads the initial value
		2. If in S Ti executes read(Q), and that value was produced by Tj, then in S’ Ti must read the value of Q that was produced by the same write(Q) operation of transaction Tj .
		3. The transaction that performs the final write(Q) operation in schedule S must also perform the final write(Q) operation in S'
schedule S’.
	Testing view serializability is NP-complete
	View serializable schedules that are not conflict serializability has **blind writes**, so view serializability is also impractical
		**blind writes**: write a resource without reading it first
### Recoverability: Atomicity & Durability
**Recoverable schedule**: if T1 reads a data item previously written by T2 , then T2 commits before T1 commits
**Cascading abort/rollback**: a single transaction failure leads to a series of transaction rollbacks
	Happens when:  a transaction T1 reads a data item previously written by a transaction T2, then T2 aborts after the read operation of T1
**Cascadeless schedule**(guaranteed to avoid cascading aborts): if T1 reads a data item previously written by T2, then T2 commits before T1 reads
	*Cascadeless* schedules are *recoverable*, and therefore safer.
# Transaction Isolation
## Typical Problems
Severity: Dirty Read > Non-repeatable Read > Phantom Read

| Problem                 | Definition                                                           | Example                                                            |
| ----------------------- | -------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **Dirty Read**          | read uncommitted data from other transactions                        | A changes data, B reads it, then A rolls back                      |
| **Non-repeatable Read** | data of a row from multiple reads are different in one transaction   | A reads the data twice, but the data is changed by B in the middle |
| **Phantom Read**        | numbers of rows from multiple reads are different in one transaction | A reads the data twice, but B adds a row in the middle             |
## Transaction Isolation Level
Isolation Level(at the cost of performance): Serializable > Repeatable Read > Read Committed > Read Uncommitted

| Isolation Level                               | Definition                                          | Potential Issues                              |
| --------------------------------------------- | --------------------------------------------------- | --------------------------------------------- |
| **Read Uncommitted**                          | Can read changes whose transaction is not committed | Dirty Read, Non-repeatable Read, Phantom Read |
| **Read Committed**                            | Only read changes whose transaction is committed    | Non-repeatable Read, Phantom Read             |
| **Repeatable Read**<br>(InnoDB default level) | Data read remains unchanged within the transaction  | Phantom Read                                  |
| **Serializable**                              | Put read/write locks on the read data               |                                               |
# Transaction Control
```SQL
COMMIT -- 提交：Commit之前的表会上锁，数据操作仅影响数据库缓存区，Commit之后保存点会被移除
ROLLBACK -- 回滚一个事务
SAVEPOINT <savepoint> -- 在一次事务中加入保存点
Roll back to <savepoint> -- 回退到保存点
```