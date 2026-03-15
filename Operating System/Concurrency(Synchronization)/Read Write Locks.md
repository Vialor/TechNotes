# Lock Based Protocols
**Locks**:
	exclusive: lock-X: only one can write
	shared: lock-S: many can read
## Two-Phase Locking 2PL
> also **Graph-Based Protocols**
### Basic 2PL
Each transaction has a **Growing Phase** to acquire locks (or convert a lock-S to a lock-X) and a **Shrinking Phase** to release locks (or convert a lock-X to a lock-S)
pros and cons: ensure conflict serializability, but can have deadlocks, and cascading rollback is possible
### Strict 2PL
2PL + transaction must hold *all exclusive locks* till it commits/aborts
additional pros: avoid cascading rollback
### Rigorous 2PL
2PL + transaction must hold *all locks* till it commits/aborts
additional pros: transactions can be serialized in the order in which they commit.
# Timestamp Based Protocols
## Deadlock
### Deadlock prevention
**wait-die**: older transaction may wait for younger one to release data item. Younger transactions never wait for older ones; they are rolled back instead.
**wound-wait**: older transaction wounds (forces rollback) of younger transaction instead of waiting for it. Younger transactions may wait for older ones.
### Deadlock detection
wait-for graph, G = (V,E):
V vertices: transactions
E edges: Ti->Tj means Ti is waiting for Tj
# Validation Based Protocols
1. Read and execution phase: Transaction Ti writes only to temporary local variables
2. Validation phase: Transaction Ti performs a validation test to determine if local variables can be written without violating serializability.
3. Write phase: If Ti is validated, the updates are applied to the database; otherwise, Ti is rolled back.