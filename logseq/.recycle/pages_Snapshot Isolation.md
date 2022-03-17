sources:: https://www.geeksforgeeks.org/snapshot-isolation-vs-serializable/
tags:: "DB Transaction Isolation Levels"

- allows the transactions occurring concurrently to see the same snapshot or copy of the database as it was at the beginning of the transactions.
- Thus, allowing a second transaction to make changes to the data that was to be read by another concurrent transaction.
- This other transaction would not observe the changes made by the second transaction and would continue working on the previous snapshot of the database.