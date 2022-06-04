tags:: distributed transaction

- require all participants in a transaction to commit or roll back before the transaction can proceed.
	- some participant implementations, such as NoSQL databases and message brokering, don't support this model.
-