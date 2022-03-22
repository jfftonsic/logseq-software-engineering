- diagram.
  [diagram elements reference](https://github.com/plantuml-stdlib/C4-PlantUML#supported-diagram-types)
	- {{renderer code_diagram,plantuml}}
		- ```plantuml
		  @startuml
		  !include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
		  
		  AddElementTag("fallback", $bgColor="#444444")
		  
		  Person(personAlias, "Label", "Optional Description")
		  Container(containerAlias, "Label", "Technology", "Optional Description")
		  System(systemAlias, "Label", "Optional Description")
		  
		  Rel(personAlias, containerAlias, "Label", "Optional Technology")
		  @enduml
		  ```
	- {{renderer code_diagram,plantuml}}
		- ```plantuml
		  @startuml
		  !include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Container.puml
		  
		  AddElementTag("fallback", $bgColor="#444444")
		  AddElementTag("extendsimplements", $lineColor="blue", $legendText="extends/implements")
		  AddElementTag("dependson", $lineColor="red", $legendText="depends on")
		  
		  
		  Boundary(cPersistenceLayer, "Persistence Layer") {
		  	ContainerDb(db, "Database", "Relational Database", "")
		  }
		  
		  
		  
		  Boundary(cJDBCInterfaces, "JDBC Interfaces\nDefined on the JVM runtime libs") {
		  	Container(cJDBCInterfacesDataSource, "javax.sql.DataSource", "java interface", "Factory for connections to the physical data source\n\nHikariDataSource\nPgSimpleDataSource\netc")
		      Container(cJDBCInterfacesConnection, "java.sql.Connection", "java interface", "A connection (session) with a specific database. SQL statements are executed and results are returned within the context of a connection.")
		  }
		  
		  
		  
		  Boundary(cJDBCImplementation, "JDBC Implementation") {
		  	Boundary(cJDBCImplementationJVM, "JVM") {
		      	Container(cJDBCImplementationJVMJavaSqlDriver, "java.sql.Driver", "Interface", "")
		      }
		      Boundary(cJDBCImplementationDbDriverLib, "Your database driver JAR") {
		      	Container(cJDBCImplementationDbDriverLibDriverClass, "DB specific JDBC driver implementation", "class", "Handles lowest-level integration with database")
		      }
		  
		  }
		  
		  Rel(cJDBCImplementation, cJDBCInterfaces, " ", $tags="extendsimplements")
		  Rel(cJDBCImplementationDbDriverLibDriverClass, cJDBCImplementationJVMJavaSqlDriver, " ", $tags="extendsimplements")
		  Rel(cJDBCImplementationDbDriverLibDriverClass, db, "interacts with")
		  
		  
		  Boundary(cJPA, "Jakarta Persistence API\nformerly JPA (Java Persistence API)") {
		  
		  	Boundary(cJPAQueryLanguage, "Query Language") {
		      	Container(cJPAQueryLanguageJPQL, "JPQL", "Java Persistence query language", "")
		      }
		  	Boundary(cJPACriteriaAPI, "Criteria API") {
		      }
		  	Boundary(cJPAORM, "Object/Relational Mapping") {
		      	Container(cJPAEntity, "javax.persistence.Entity", "java annotation", "Lightweight persistence domain object")
		          Container(cJPAEntityManager, "javax.persistence.EntityManager", "java interface", "interact with the persistence context")
		          Container(cJPAPersistenceUnit, "Persistence Unit", "", "Defines a set of all entity classes that are managed by EntityManager instances in an application.\n\nThis set of entity classes represents the data contained within a single data store.")
		      	Container(cJPAEntityTransaction, "javax.persistence.EntityTransaction", "java interface", "control transactions on resource-local entity managers")
		      }
		  }
		  
		  
		  
		  Boundary(cJTA, "Jakarta Transaction API\nformerly JTA (Java Transaction API)") {
		  	Container(cJTATransaction, "javax.transaction.Transaction", "java interface", "Allows operations to be performed against the transaction in the target Transaction object")
		  }
		  
		  
		  
		  Boundary(cHibernate, "Hibernate") {
		  	Container(cHibernateSession, "org.hibernate.Session", "java interface", "A single-threaded, short-lived object conceptually modeling a Unit of Work")
		      Rel(cHibernateSession, cJPAEntityManager, " ", $tags="extendsimplements")
		  
		      Container(cHibernateTransaction, "org.hibernate.Transaction", "java interface", "")
		      Rel(cHibernateTransaction, cJPAEntityTransaction, " ", $tags="extendsimplements")
		  }
		  Rel(cHibernate, cJDBCInterfaces, "uses", "")
		  Rel(cHibernate, cJPA, " ", $tags="extendsimplements")
		  
		  
		  
		  
		  Boundary(cSpring, "Spring") {
		  
		  	Boundary(cSpringJdbc, "Spring JDBC") {
		      	Container(cSpringJdbcJdbcTemplate, "org.springframework.jdbc.core.JdbcTemplate", "java class", "")
		          Container(cSpringJdbcDSImpls, "implementations for DataSources", " ")
		      	Rel(cSpringJdbcDSImpls, cJDBCInterfacesDataSource, " ", $tags="extendsimplements")
		      }
		  
		      Boundary(cSpringDataCommons, "Spring Data Commons") {
		      	Container(cSpringDataCommonsJar, "spring-data-commons.jar", "", "Provides for example:\nRepository\nCrudRepository\nPagingAndSortingRepository")
		  
		      }
		  
		  	Boundary(cSpringOrm, "Spring ORM") {
		      	Container(cSpringOrmJar, "spring-orm.jar", " ")
		          Rel(cSpringOrmJar, cHibernate, "uses", "")
		      }
		      Rel(cSpringOrm, cSpringJdbc, " ", $tags="dependson")
		  
		      Boundary(cSpringTransactions, "Spring Transactions") {
		      	Container(cSpringTransactionsJar, "spring-tx.jar")
		      }
		      Rel(cSpringTransactions, cJTA, " ", $tags="dependson")
		  
		  
		      Boundary(cSpringDataJpa, "Spring Data JPA") {
		      	Container(cSpringDataJpaJar, "spring-data-jpa.jar", "", "Provides repository support for the JPA\n\nJpaRepository\n@Lock")
		      }
		      Rel(cSpringDataJpa, cSpringOrm, " ", $tags="dependson")
		      Rel(cSpringDataJpa, cSpringDataCommons, " ", $tags="dependson")
		  }
		  
		  
		  SHOW_LEGEND()
		  
		  @enduml
		  ```
		- ```
- dsfas
-