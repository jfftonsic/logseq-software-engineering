- The exception
  `javax.persistence.OptimisticLockException: Row was updated or deleted by another transaction (or unsaved-value mapping was incorrect) : [com.example.db.relational.entity.BalanceUpdateReservationEntity#e25f230e-ffa8-4116-b9c0-da765cee1558]`
  was thrown for the following code section
	- ```java
	      @Override
	      @Transactional(
	              timeout = TIMEOUT_MS,
	              isolation = Isolation.READ_COMMITTED,
	              noRollbackFor = NotEnoughBalanceException.class
	      )
	      public String createUpdateReservation(/*...*/) throws NotEnoughBalanceException {
	          //...
	          BalanceUpdateReservationEntity balanceUpdateReservationEntity = BalanceUpdateReservationEntity.builder()
	                  //...
	                  .build();
	          balanceUpdateReservationEntity = balanceUpdateReservationRepository.save(balanceUpdateReservationEntity);
	  
	          // The line below throws the exception
	          entityManager.lock(balanceUpdateReservationEntity, LockModeType.PESSIMISTIC_WRITE);
	          ///...
	      }
	  ```
	-
-