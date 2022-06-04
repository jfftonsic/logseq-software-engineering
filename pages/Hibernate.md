tags:: #Frameworks

- Extra topics
  heading:: true
	- [[Hibernate and entity self-references]]
	- [[Hibernate one-to-one associations and laziness]]
-
- Some fast, important points
  heading:: true
	- Never use a List if you model a Many-to-Many association. Use a Set.
	  collapsed:: true
		- Hibernate handles remove operations on Many-to-Many relationships that are mapped to a java.util.List very inefficiently.
		- example
			- ```java
			  @Entity
			  public class Book {
			   
			      // DON'T DO THIS!!!
			      @ManyToMany
			      @JoinTable(name = "book_author", 
			              joinColumns = { @JoinColumn(name = "fk_book") }, 
			              inverseJoinColumns = { @JoinColumn(name = "fk_author") })
			      private List<Author> authors = new ArrayList<Author>();
			       
			      ...
			  }
			  ```
		-
	- Avoid at all costs CascadeTypes REMOVE and ALL
-
- Architecture
  heading:: true
	- ![image.png](../assets/image_1647302053648_0.png)
- `org.hibernate.SessionFactory`
  heading:: true
  collapsed:: true
	- You would need one `SessionFactory` object per database using a separate configuration file.
		- if you are using multiple databases
			- create multiple `SessionFactory` objects.
	- The internal state of a `SessionFactory`
		- is **immutable**.
		- includes **all of the metadata about Object/Relational Mapping**.
-
- [[Transaction]]-related
  heading:: true
	- important classes:
		- `org.hibernate.Session`
		  id:: 622fd6c4-e34c-47af-b549-1f55f7ba6fab
		  alias:: Session
			- used to get a physical connection with a database
			- is lightweight and designed to be instantiated each time an interaction is needed with the database.
			- The session objects should not be kept open for a long time because they are not usually thread safe and they should be created and destroyed them as needed.
			- The lifecycle of a Session is bounded by the beginning and end of a logical transaction. (Long transactions might span several database transactions.)
			- The main function of the Session is to offer create, read and delete operations for instances of mapped entity classes. Instances may exist in one of three states:
				- **transient**: never persistent, not associated with any Session
				- **persistent**: associated with a unique Session
				- **detached**: previously persistent, not associated with any Session
			- Changes to persistent instances are detected at flush time and also result in an SQL UPDATE.
			- If the Session throws an exception, the transaction must be rolled back and the session discarded.
			- sample code:
			  collapsed:: true
				- ```java
				     Session sess = factory.openSession();
				     Transaction tx;
				     try {
				         tx = sess.beginTransaction();
				         //do some work
				         ...
				         tx.commit();
				     }
				     catch (Exception e) {
				         if (tx!=null) tx.rollback();
				         throw e;
				     }
				     finally {
				         sess.close();
				     }
				  ```
		- `org.hibernate.Transaction`
			- A transaction is associated with a ((622fd6c4-e34c-47af-b549-1f55f7ba6fab)) and is usually initiated by a call to `Session.beginTransaction()`.
			- A single session might span multiple transactions since the notion of a session (a conversation between the application and the datastore) is of coarser granularity than the notion of a transaction. However, it is intended that there be at most one uncommitted transaction associated with a particular Session at any time.
		- `org.hibernate.engine.transaction.internal.TransactionImpl`
			- #+BEGIN_TIP
			  If you want some interesting debugging logs, configure DEBUG logger for this class. It will print transaction creations, commits, rollbacks
			  #+END_TIP
		- `org.hibernate.resource.jdbc.internal.AbstractLogicalConnectionImplementor`
		  id:: 622fd7e9-d5c5-49aa-bef5-acf9c7d419da
			- collapsed:: true
			  #+BEGIN_TIP
			  If you want some interesting debugging logs, configure TRACE logger for this class. It will log lots of important things, see a list on the potentially collapsed sub-block
			  #+END_TIP
				- ```text
				  LogicalConnection#afterStatement
				  LogicalConnection#beforeTransactionCompletion
				  LogicalConnection#afterTransaction
				  Preparing to begin transaction via JDBC Connection.setAutoCommit(false)
				  Transaction begun via JDBC Connection.setAutoCommit(false)
				  Preparing to commit transaction via JDBC Connection.commit()
				  Transaction committed via JDBC Connection.commit()
				  re-enabling auto-commit on JDBC Connection after completion of JDBC-based transaction
				  Could not re-enable auto-commit on JDBC Connection after completion of JDBC-based transaction
				  Preparing to rollback transaction via JDBC Connection.rollback()
				  Transaction rolled-back via JDBC Connection.rollback()
				  Unable to ascertain initial auto-commit state of provided connection; assuming auto-commit
				  ```
-
- Named queries
  heading:: true
  collapsed:: true
	- naming strategy
		- you pass your custom name where you use the query (which seems less prone to refactory problems.)
		- if you don't want to have to specify a name, the default is this syntax: `[entity class name].[name of the invoked query method]`.
	- through properties file
		- you can put the query on spring boot properties file
		- or [this post](https://www.petrikainulainen.net/programming/spring-framework/spring-data-jpa-tutorial-creating-database-queries-with-named-queries/) says
		  > We can declare named queries by adding them into the`jpa-named-queries.properties`  file that is found from the `META-INF` folder of our classpath.
		- #+BEGIN_NOTE
		  I could not make it work with Spring Boot's `application.yml`.
		  But with `jpa-named-queries.properties` mentioned before, it worked.
		  #+END_NOTE
	- through annotations
		- one advantage I can see: the query stays as close as possible as the structure of the data
		- `@NamedQuery`, `@NamedNativeQuery`
		  ```java
		  @Entity
		  @NamedQuery(name = "Todo.findByTitleIs",
		          query = "SELECT t FROM Todo t WHERE t.title = 'title'"
		  )
		  @Table(name = "todos")
		  final class Todo {
		       
		  }
		  ```
	- seems like if your repository's method parameter has the same name as the named parameter you use inside your query, it works. If it doesn't, or the name differs, you can specify it, e.g.:
	  ```java
	  List<Todo> findBySearchTermNamed(@Param("searchTerm") String searchTerm);
	  ```
-
- ResultSet mapping
  heading:: true
  collapsed:: true
	- When using custom native queries and you want to map it to an entity:
	  
	  
	  
	  ```java
	  
	      Query q = em.createNativeQuery(
	          "SELECT o.id AS order_id, " +
	              "o.quantity AS order_quantity, " +
	              "o.item AS order_item, " +
	              "i.name AS item_name, " +
	          "FROM Order o, Item i " +
	          "WHERE (order_quantity > 25) AND (order_item = i.id)",
	      "OrderResults");
	      
	      @SqlResultSetMapping(name="OrderResults", 
	          entities={ 
	              @EntityResult(entityClass=com.acme.Order.class, fields={
	                  @FieldResult(name="id", column="order_id"),
	                  @FieldResult(name="quantity", column="order_quantity"), 
	                  @FieldResult(name="item", column="order_item")})},
	          columns={
	              @ColumnResult(name="item_name")}
	      )
	  ```
	  
	  
	  #+BEGIN_PINNED
	  <mark style="background-color: orange">I don't know what happens to `item_name` in the example above since it is not a field on Order class.</mark>
	  #+END_PINNED
-
- Interesting errors
  heading:: true
  collapsed:: true
	- `org.postgresql.util.PSQLException: ERROR: FOR UPDATE cannot be applied to the nullable side of an outer join`
		- Happened when I tried to lock 2 rows in separate tables when one of the sides of the left join didn't have result.
		  The query in that case was:
		  ```java
		  entityManager.createQuery(
		                          "SELECT b, c from balanceUpdateReservation b left join balanceUpdateConfirm c on c.balanceUpdateReservationEntity.id = b.id WHERE b.reservationCode = :reservationCode")
		                  .setLockMode(LockModeType.PESSIMISTIC_WRITE).setParameter("reservationCode", reservationCode)
		    .getResultList()
		  ```
		  and, for the `reservationCode` given, there were no `balanceUpdateConfirm`