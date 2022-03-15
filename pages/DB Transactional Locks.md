- on most RDBMS, explicit physical locks (SELECT ... FOR UPDATE) can only prevent record modifications for database records that existed at the time of locking while **future records can be added**.
  sources:: https://vladmihalcea.com/how-does-database-pessimistic-locking-interact-with-insert-update-and-delete-sql-statements/
	- image example:
		- ![image.png](../assets/image_1647288115213_0.png)
		  Bob’s INSERT statement is executed right away even if Alice’s transaction tried to lock all PostComment entries.
		-