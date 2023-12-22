# Definition
- Relational database that uses SQL and organized using table, rows, and columns.
	- Table represents 1 entities of the database
	- Column represents many attributes of that entities
	- Each row represent 1 entry of data of that entities

[]()## ACID
- A set of properties of relational database transactions (A way of representing a state change)
- **Atomic**: 
	- In general, *atomic* refers to something that cannot be broken down into smaller parts. 
	- *Atomic* describes what happens if a client wants to make several writes, but a fault occurs after some of the writes have been processed. If the writes are grouped together into an atomic transaction, and the transaction cannot be completed (*committed*) due to a fault, then the transaction is aborted and the database must discard or under any writes it has made so far in that transaction
	- Without atomicity, if an error occurs partway through making multiple changes, it's difficult to know which changes have taken effect and which haven't. The application could try again, but that risks making the same change twice, leading to duplicate or incorrect data.
	- *Summary*: *Atomic* is the ability to abort a transaction on error and have all writes from that transaction discarded
- **Consistent**: 
	- There are certain statements about your data that must always be true. E.g in an accounting system, credits and debits across all account must always be balanced. If a transaction starts with a database that is valid according to these invariants, and any writes during the transaction preserver the validity, then you can be sure that the invariants are always satisfies.
	- However, the idea of consistency depends on the application's notion of invariant, and it's the application's responsibility to define its transactions correctly so that they preserve consistency.
	- *Concistency* is a property of the application. The application may rely on the database's *atomicity* and *isolation* to achieve consistency, but it's not up to the database alone.
- **Isolation**: Concurrent execution of transactions leaves the database in the same state that would have been obtained if the transactions were executed sequentially.
	- Ref: https://en.wikipedia.org/wiki/ACID#Isolation_failure, https://retool.com/blog/isolation-levels-and-locking-in-relational-databases/
	- Concurrently executing transactions are isolated from each other: they cannot step on each other's toes.
	- The classic database isolation level is *serializability*, meaning that each transaction can pretend that it is the only transaction running on the entire database. The database ensure that when the transactions have committed, the result is the same as if they had run *serially*, even though they may have run conncurently
- **Durability**: Once the change has happened, if the system says the transaction has been committed, the client doesn't need to worry about "flushing" the system to make the change stick.
	- The data after being *committed* has been written to a nonvolatile storage, usually involves a  write-ahead log, which allows for recovery in the event that the data structures on disk are corrupted.
	- In a replicated database, durability may mean that the data has been successfully copied to some number of nodes. In order to provide a durability guarantee, a database must wait until these writes or replications are complete before reporting a transactions as succesfully committed.

