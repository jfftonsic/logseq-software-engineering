alias:: Optimistic Concurrency Control Technique

- Short description
	- The local copies of the transaction data are updated rather than the data itself. Just before the changes are to be applied, the data is checked for violation of serializability.
- results in less interference while execution of the transaction.
- Read Phase
	- Data values from the database can be read by a transaction but the write operation or updates are only applied to the local data copies, not the actual database.
- Validation Phase
	- Data is checked to ensure that there is no violation of serializability while applying the transaction updates to the database.
- Write Phase
	- Updates are applied to the database if the validation is successful, else; the updates are not applied, and the transaction is rolled back.