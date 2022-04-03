sources:: https://attacomsian.com/blog/spring-data-jpa-query-annotation, https://attacomsian.com/blog/derived-query-methods-spring-data-jpa

- Dynamic Queries with SpEL Expressions
  collapsed:: true
	- Generic Entity Names
	  ```java
	  @Query("SELECT n from #{#entityName} n WHERE n.title = ?1")
	  List<Note> findByTitleGeneric(String title);
	  ```
	- Advanced LIKE Expressions
	  ```java
	  @Query("SELECT n FROM Note n WHERE lower(n.title) LIKE %?#{[0].toLowerCase()}%")
	  List<Note> findByTitleIgnoreCaseSpEL(String title);
	  ```
	-
- Derived Query Methods in Spring Data JPA
	- method name has two main components separated by the first `By` keyword
		- The introducer clause like `find`, `read`, `query`, `count`, or `get`
			- which tells Spring Data JPA what you want to do with the method. This clause can contain further expressions, such as `Distinct` to set a distinct flag on the query to be created.
			- You can also use `readBy`, `getBy`, and `queryBy` in place of `findBy`
		- The criteria clause that starts after the first By keyword. The first By acts as a delimiter to indicate the start of the actual query criteria. The criteria clause is where you define conditions on entity properties and concatenate them with And and Or keywords.
	- examples
		- ```java
		  // find users by last name
		  List<User> findByLastName(String lastName);
		  
		  // find distinct users by email
		  List<User> findDistinctByEmail(String email);
		  
		  // count users by profession
		  int countByProfession(String profession);
		  
		  List<User> findByNameOrEmail(String name, String email);
		  List<User> findByNameAndAge(String name, int age);
		  List<User> findByActiveAndBirthDateOrNameAndAge(boolean active,Date dob, String name, int age);
		  
		  // To express the inequality
		  List<User> findByNameIsNot(String name);
		  // OR
		  List<User> findByNameNot(String name);
		  
		  List<User> findByEmailIsNull();
		  List<User> findByEmailIsNotNull();
		  
		  List<User> findByActiveTrue();
		  List<User> findByActiveFalse();
		  
		  // ... WHERE name LIKE 'prefix%'
		  List<User> findByNameStartingWith(String prefix);
		  // OR
		  List<User> findByNameIsStartingWith(String prefix);
		  // OR
		  List<User> findByNameStartsWith(String prefix);
		  
		  // ... WHERE name LIKE '%suffix'
		  List<User> findByNameEndingWith(String suffix);
		  // ... WHERE name LIKE '%infix%'
		  List<User> findByNameContaining(String infix);
		  
		  //String pattern = "%atta%@gmail%";
		  List<User> findByEmail[Not]Like(String pattern);
		  
		  List<User> findByAge[Less/Greater]Than(int age);
		  List<User> findByAge[Less/Greater]ThanEqual(int age);
		  List<User> findByAgeBetween(int start, int end);
		  List<User> findByBirthDateBefore(Date before);
		  List<User> findByBirthDateAfter(Date after);
		  
		  List<User> findDistinctByEmail(String email);
		  
		  List<User> findDistinctPeopleByNameOrEmail(String name, String email);
		  
		  List<User> findByNameIgnoreCase(String name);
		  //To enable case-insensitive search for all suitable properties
		  List<User> findByNameOrEmailAllIgnoreCase(String name, String email);
		  
		  // Static ordering
		  List<User> findByNameContainingOrderByName(String name);
		  // OR
		  List<User> findByNameContainingOrderByName[Asc/Desc](String name);
		  
		  // Dynamic ordering
		  List<User> findByNameContaining(String name, Sort sort);
		  // multiple sort parameters
		  List<User> users = userRepository.findByNameContaining("john", Sort.by("name", "age").descending());
		  
		  //Limiting Derived Query Results
		  User findFirstByOrderByName();
		  User findTopByOrderByAgeDesc();
		  List<User> findFirst5ByEmail(String email);
		  List<User> findDistinctTop3ByAgeLessThan(int age);
		  
		  //Paginate Derived Query Results
		  Page<User> findByActive(boolean active, Pageable pageable);
		  Pageable pageable = PageRequest.of(0, 10);
		  Page<User> userPage = userRepository.findByActive(true, pageable)
		  Pageable pageable = PageRequest.of(0, 10, Sort.by("name").descending());
		  Page<User> userPage = userRepository.findByActive(true, pageable);
		  ```
-
- Spring Data JPA
	- https://docs.spring.io/spring-data/jpa/docs/current/reference/html/
	- https://www.baeldung.com/the-persistence-layer-with-spring-data-jpa
	- https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#orm
-
- Spring JDBC
	- https://www.baeldung.com/spring-data-jdbc-intro
	- https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#data.sql.jdbc
	- https://docs.spring.io/spring-framework/docs/current/reference/html/data-access.html#jdbc
-