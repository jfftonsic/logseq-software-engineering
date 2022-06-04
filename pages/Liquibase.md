- Bizarre errors
	- ```
	  Caused by: liquibase.exception.ValidationFailedException: Validation Failed:
	       1 changes have validation failures 
	       	AS is not allowed on postgresql ...
	  ```
		- Why it happens?
			- You are not allowed to set a sequence datatype if you are using a postgres with version < 10
			- ```yml
			  - createSequence:
			      cacheSize: 1
			      dataType: bigint   # THIS HERE
			      incrementBy: 50
			      schemaName: public
			      sequenceName: seq_emp_id
			      startValue: 1
			  ```
		- Where in Liquibase source code this appears
			- The error gets added at CreateSequenceGenerator
			- ```java
			  if (isPostgreWithoutAsDatatypeSupport(database)) {
			  	validationErrors.checkDisallowedField("AS", statement.getDataType(), database, PostgresDatabase.class);
			  }
			  ```