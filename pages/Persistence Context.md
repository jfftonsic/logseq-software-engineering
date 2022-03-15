sources:: [EntityManager javadoc](https://docs.oracle.com/javaee/7/api/javax/persistence/EntityManager.html), [Baeldung - JPA/Hibernate Persistence Context](https://www.baeldung.com/jpa-hibernate-persistence-context)

- in [[Java]]
	- A persistence context is **a set of entity instances in which for any persistent entity identity there is a unique entity instance**. Within the persistence context, the entity instances and their lifecycle are managed.
	- The persistence context is the first-level cache where all the entities are fetched from the database or saved to the database.
	- If anything changes during a transaction, then the entity is marked as dirty
	- Persistence contexts are available in two types:
	- [[Transaction-scoped persistence context]]
	- [[Extended-scoped persistence context]]