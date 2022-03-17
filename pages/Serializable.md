sources:: https://www.geeksforgeeks.org/snapshot-isolation-vs-serializable/
tags:: DB Transaction Isolation Levels

- Transactions appears as if they are running in a serial order.
- Makes use of locks for reading and writing operations.
- A [[Lock]] makes sure that no other concurrent transaction is allowed to amend the data used by the transaction holding the lock until it gets completed.