- [Spring Transaction Management: @Transactional In-Depth](https://www.marcobehler.com/guides/spring-transaction-management-transactional-in-depth)
- Spring Transaction Abstractions
	- `org.springframework.transaction.PlatformTransactionManager`
	  collapsed:: true
		- ```java
		  public interface PlatformTransactionManager {
		     //This method returns a currently active transaction or creates a new one, according to the specified propagation behavior.
		     TransactionStatus getTransaction(TransactionDefinition definition);
		     throws TransactionException;
		     
		     void commit(TransactionStatus status) throws TransactionException;
		     void rollback(TransactionStatus status) throws TransactionException;
		  }
		  ```
	- `TransactionDefinition` is the core interface of the transaction support
	  collapsed:: true
		- ```java
		  public interface TransactionDefinition {
		     int getPropagationBehavior();
		     int getIsolationLevel();
		     String getName();
		     int getTimeout();
		     boolean isReadOnly();
		  }
		  ```
	- `TransactionStatus` interface provides a simple way for transactional code to control transaction execution and query transaction status.
	  collapsed:: true
		- ```java
		  public interface TransactionStatus extends SavepointManager {
		     boolean isNewTransaction();
		     boolean hasSavepoint();
		     void setRollbackOnly();
		     boolean isRollbackOnly();
		     boolean isCompleted();
		  }
		  ```
- The `isolation` parameter at the `@Transactional` **only applies to newly started transactions**. Switch the `validateExistingTransactions` flag to `true` on your transaction manager if you'd like isolation level declarations to get **rejected** when **participating in an existing transaction with a different isolation level.**
- [Should my tests be @Transactional?](https://www.marcobehler.com/2014/06/25/should-my-tests-be-transactional)
- Spring Transaction Abstractions
	- `TransactionDefinition`: propagation, isolation, timeout, read-only flag.
	- `PlatformTransactionManager`: commit, rollback
- {{renderer code_diagram,nomnoml}}
	- ```nomnoml
	  [Pirate|eyeCount: Int|raid();pillage()|
	    [beard]--[parrot]
	    [beard]-:>[foul mouth]
	  ]
	  [<table>mischief 
	  | bawlfd sf sadf abawlfd sf sadf abawlbawlfd sf sadf abawlfd sf sadf abawlfd sf sadf afd sf sadf a | bawlfd sf sadf abawlfd sf sadf abawlfd sf sadf abawlfd sf sadf abawlfd sf sadf abawlfd sf sadf a ||
	  yell | drink
	  ]
	  ```
- {{renderer :tables_npozuslk}}
	- data nosum
		- bawl ea sdfagidf asdfg aisudf asiuhf ashdf asidfh aius
			- dfasdf
		- dfasdf df auihdf aihdf usduif hasdfhua suhd faiui
			- dsfasdfas
		- dfasdf df auihdf aihdf usduif hasdfhua suhd faiui
			- dsfasdfas
		- dfasdf df auihdf aihdf usduif hasdfhua suhd faiui
			- dsfasdfas
	-