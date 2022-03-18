sources:: https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/autoconfigure/orm/jpa/DataJpaTest.html

- Annotation for a JPA test that focuses only on JPA components.
- By default
	- tests annotated with @DataJpaTest are **transactional and roll back at the end of each test**.
	- **use an embedded in-memory database (replacing any explicit or usually auto-configured DataSource)**.
		- The `@AutoConfigureTestDatabase` annotation can be used to override these settings.
- SQL queries are **logged by default by setting the spring.jpa.show-sql property to true**. This can be disabled using the showSql attribute.
- #+BEGIN_TIP
  If you are looking to load your full application configuration, but use an embedded database, you should consider @[[SpringBootTest]] combined with `@AutoConfigureTestDatabase` rather than this annotation.
  #+END_TIP
- [[Junit]] sample:
	- #+BEGIN_TIP
	  We can, for instance, inject a DataSource, @JdbcTemplate or @EntityManager into our test class if we need them.
	  #+END_TIP 
	  sources:: https://reflectoring.io/spring-boot-data-jpa-test/
	- ```java
	  @DataJpaTest
	  class UserEntityRepositoryTest {
	  
	    @Autowired private DataSource dataSource;
	    @Autowired private JdbcTemplate jdbcTemplate;
	    @Autowired private EntityManager entityManager;
	    @Autowired private UserRepository userRepository;
	  
	    @Test
	    void injectedComponentsAreNotNull(){
	      assertThat(dataSource).isNotNull();
	      assertThat(jdbcTemplate).isNotNull();
	      assertThat(entityManager).isNotNull();
	      assertThat(userRepository).isNotNull();
	    }
	  }
	  ```
-
- Creating the Database Schema
	- Using Hibernate’s ddl-auto
		- spring.jpa.hibernate.ddl-auto
			- create-drop by default, meaning that the schema is created before running the tests and dropped after the tests have executed.
	- Using schema.sql
		- If Spring finds a schema.sql file in the classpath, this will be executed against the datasource. This overrides the `ddl-auto` configuration of Hibernate discussed above.
		- We can control whether the schema.sql file should be executed with the property `spring.datasource.initialization-mode`. The default value is `embedded`, meaning it will only execute for an embedded database (i.e. in our tests). If we set it to `always`, it will always execute.
		- It makes sense to set Hibernate’s `ddl-auto` configuration to `validate` when using a script to initialize the schema, so that Hibernate checks if the created schema matches the entity classes on startup:
			- ```java
			  @ExtendWith(SpringExtension.class)
			  @DataJpaTest
			  @TestPropertySource(properties = {
			          "spring.jpa.hibernate.ddl-auto=validate"
			  })
			  class SchemaSqlTest {
			    ...
			  }
			  ```
	- Using [[Liquibase]]
		- [[Spring Boot + Liquibase]]
-
- Populating the Database
	- Inserting Entities Manually
		- big advantage: it’s refactoring-safe.
			- Changes in the codebase lead to compile errors in our test code.
		- disadvantage: Creating objects with complex relationships becomes tiresome.
		- sample
			- ```java
			  @Test
			  void whenSaved_thenFindsByName() {
			    userRepository.save(new UserEntity(
			            "Zaphod Beeblebrox",
			            "zaphod@galaxy.net"));
			    assertThat(userRepository.findByName("Zaphod Beeblebrox")).isNotNull();
			  }
			  ```
			-
	- Using `@Sql`
		- sample
			- sql
				- ```sql
				  -- createUser.sql
				  INSERT INTO USER 
				              (id, 
				               NAME, 
				               email) 
				  VALUES      (1, 
				               'Zaphod Beeblebrox', 
				               'zaphod@galaxy.net'); 
				  ```
			- java
				- ```java
				  @DataJpaTest
				  class SqlTest {
				  
				    @Autowired
				    private UserRepository userRepository;
				  
				    @Test
				    @Sql("createUser.sql") // If we need more than one script, we can use @SqlGroup to combine them.
				    void whenInitializedByDbUnit_thenFindsByName() {
				      UserEntity user = userRepository.findByName("Zaphod Beeblebrox");
				      assertThat(user).isNotNull();
				    }
				  
				  }
				  ```
	- Using data.sql
		- #+BEGIN_CAUTION
		  not a good approach
		  #+END_CAUTION
			- A data.sql file forces us to put all our insert statements into a single place. Every single test will depend on this one script to set up the database state. This script will soon become very large and hard to maintain
	- Using [[Spring DBUnit]]
	  sources:: https://springtestdbunit.github.io/spring-test-dbunit/index.html, http://dbunit.sourceforge.net/
	  collapsed:: true
		- {{embed ((62339a8a-f6ad-4ad2-ab1d-769cf782058e))}}
		- #+BEGIN_WARNING
		  Seems like a dead project
		  #+END_WARNING
		- sample from https://reflectoring.io/spring-boot-data-jpa-test/
			- dependencies:
			  ```groovy
			  compile('com.github.springtestdbunit:spring-test-dbunit:1.3.0')
			  compile('org.dbunit:dbunit:2.6.0')
			  ```
			  for each test, create a custom XML file (By default, the XML file (let’s name it createUser.xml) lie in the classpath next to the test class):
			  ```xml
			  <?xml version="1.0" encoding="UTF-8"?>
			  <dataset>
			      <user
			          id="1"
			          name="Zaphod Beeblebrox"
			          email="zaphod@galaxy.net"
			      />
			  </dataset>
			  ```
			  then the java
			  ```java
			  @DataJpaTest
			  @TestExecutionListeners({
			          DependencyInjectionTestExecutionListener.class,
			          TransactionDbUnitTestExecutionListener.class
			  })
			  class SpringDbUnitTest {
			  
			    @Autowired
			    private UserRepository userRepository;
			  
			    @Test
			    @DatabaseSetup("createUser.xml")
			    void whenInitializedByDbUnit_thenFindsByName() {
			      UserEntity user = userRepository.findByName("Zaphod Beeblebrox");
			      assertThat(user).isNotNull();
			    }
			  
			  }
			  ```
			  For testing queries that change the database state we could even use `@ExpectedDatabase` to define the state the database is expected to be in after the test.
		- the library page i-frame (expand block to see):
		  collapsed:: true
			- <iframe src="https://springtestdbunit.github.io/spring-test-dbunit/index.html" height="800px"/>