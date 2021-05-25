# Chapter 7 Transactions

**A transaction** is a way for an application to group several reads and writes together into a logical unit. Conceptually, all the reads and writes in a transaction are executed as one operation: either the entire transaction succeeds \(commit\) or it fails \(abort, rollback\). In other words, if a transaction was aborted, the application can be sure that it didn't change anything.



Transactions-&gt; think wechat transaction 



### ACID - Atomicity Consistency Isolation Durability

**Atomic** refers to something that cannot be broken down into smaller pieces. 

ACID **consistency** is that you have certain statements about your data\(invariants\) that must be always true

**Isolation** in the sense of ACID means that concurrently executing transactions are isolated from each other: they cannot step on each other's toes.  

**Durability** is the promise that once a transaction has committed successfully, any data it has written will not be forgotten, even if there is a hardware fault or the database crashes.



