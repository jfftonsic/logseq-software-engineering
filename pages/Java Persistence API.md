alias:: JPA
sources:: https://www.baeldung.com/java-jpa-transaction-locks

- Each JPA implementation provides some type of [[object-relation mapping]] layer
- JPA Lock Types
	- Pessimistic Locking
		- **When the transaction access an entity, it will be locked immediately** throwing a `LockTimeoutException` if it fails. If you want to avoid that and specify a lock timeout, then:
		  ```java
		  @Lock(LockModeType.PESSIMISTIC_READ)
		  @QueryHints({@QueryHint(name = "javax.persistence.lock.timeout", value = "3000")})
		  public Optional<Customer> findById(Long customerId);
		  ```
	- Optimistic Locking
	  collapsed:: true
		- **Transaction doesn't lock the entity immediately**. Instead, the transaction commonly saves the entity's state with a version number assigned to it.
		  
		  When we try to update the entity's state in a different transaction, the transaction compares the saved version number with the existing version number during an update.
		  
		  At this point, if the version number differs, it means that the entity can't be modified. If there is an active transaction then that transaction will be rolled back and the underlying JPA implementation will throw an [[OptimisticLockException]] .
		  
		  Apart from the version number approach, we can use other approaches such as timestamps, hash value computation, or serialized checksum, depending on which approach is the most suitable for our current development context.
- When the lock is **explicitly enabled** and there is **no active transaction**, the underlying JPA implementation will throw a `TransactionRequiredException`.
- In case the lock cannot be granted and the locking conflict doesn't result in a transaction rollback, JPA throws a `LockTimeoutException`. But it **doesn't mark the active transaction for rollback**.
- Returning more than one entity in a query
  collapsed:: true
	- ```java
	         final var namedQuery = entityManager.createQuery(
	                          """
	                                  SELECT b, c
	                                  FROM balanceUpdateReservation b, balanceUpdateConfirm c
	                                  WHERE b.reservationCode = :reservationCode AND c.balanceUpdateReservationEntity.id = b.id
	                                  """);
	  
	          namedQuery.setParameter("reservationCode", reservationCode);
	  
	          final var resultList = namedQuery.getResultList();
	           
	           // if 1 row of b and one row of c satisfies the query
	           // this resultList will be an ArrayList with 1 Object[2] instance
	           // Object[0] will be a balanceUpdateReservationEntity and 
	           // Object[1] will be a balanceUpdateConfirmEntity
	  ```
	  #jpa #sample