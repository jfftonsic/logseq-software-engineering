sources:: https://www.baeldung.com/spring-boot-testcontainers-integration-test
alias:: Testcontainer

- Sample JUnit also with [[Liquibase]]
  collapsed:: true
  tags:: Junit, Liquibase
	- ```java
	  package com.example;
	  
	  import liquibase.Contexts;
	  import liquibase.Liquibase;
	  import liquibase.database.Database;
	  import liquibase.database.DatabaseFactory;
	  import liquibase.database.jvm.JdbcConnection;
	  import liquibase.resource.FileSystemResourceAccessor;
	  import org.junit.jupiter.api.BeforeEach;
	  import org.junit.jupiter.api.Test;
	  import org.testcontainers.containers.PostgreSQLContainer;
	  import org.testcontainers.junit.jupiter.Container;
	  import org.testcontainers.junit.jupiter.Testcontainers;
	  
	  import java.nio.file.Files;
	  import java.nio.file.Paths;
	  import java.sql.Connection;
	  import java.sql.DriverManager;
	  
	  @Testcontainers
	  public class DatabaseTest {
	  
	      // will be started before and stopped after each test method
	      @Container
	      private PostgreSQLContainer postgresqlContainer = new PostgreSQLContainer("postgres")
	              .withDatabaseName("postgres")
	              .withUsername("postgres")
	              .withPassword("a");
	      private static Connection conn;
	      private static Liquibase liquibase;
	  
	      @BeforeEach
	      public void setUp() throws Exception {
	          Class.forName("org.postgresql.Driver");
	  
	          conn = DriverManager.getConnection(postgresqlContainer.getJdbcUrl()/*"jdbc:postgresql://localhost:5432/"*/,
	                  postgresqlContainer.getUsername(),
	                  postgresqlContainer.getPassword());
	  
	          Database database = DatabaseFactory.getInstance()
	                  .findCorrectDatabaseImplementation(new JdbcConnection(conn));
	  
	          String currentDirectory = System.getProperty("user.dir");
	          System.out.println("The current working directory is " + currentDirectory);
	          System.out.println("The file exists? " + Files.exists(Paths.get("./src/main/resources/db/changelog/db.changelog-master.yaml")));
	          liquibase = new Liquibase("./src/main/resources/db/changelog/db.changelog-master.yaml",
	                  new FileSystemResourceAccessor(Paths.get(currentDirectory).toFile()), database);
	          liquibase.update((Contexts) null);
	  
	          System.out.println();
	      }
	  
	      @Test
	      public void testSimplePutAndGet() {
	          postgresqlContainer.isRunning();
	      }
	  }
	  
	  ```
- Sample spring datasource configuration that itself tells that the context should bring up a testcontainer as the database
  tags:: Spring, Spring Boot, Junit, Testing
  collapsed:: true
	- ```yml
	  # at application-integrationTest.yml for example
	  spring:
	    datasource:
	      hikari:
	        auto-commit: false
	      driver-class-name: org.testcontainers.jdbc.ContainerDatabaseDriver
	      url: jdbc:tc:postgresql:13.3:///
	      username: postgres
	  
	    jpa:
	      database-platform: org.hibernate.dialect.PostgreSQL9Dialect
	      hibernate:
	        ddl-auto: none
	      properties:
	        hibernate:
	          default-schema: public
	    test:
	      database:
	        replace: none
	  ```
	- Then at the test class for example
		- ```java
		  @SpringBootTest
		  @ActiveProfiles("integrationTest")
		  @AutoConfigureMockMvc
		  public class EmployeeHierarchyFullEnvIT {
		      @Autowired
		      private MockMvc mockMvc;
		  
		      @Autowired
		      private JdbcTemplate jdbcTemplate;
		  
		    // ...
		    
		  }
		    
		  ```
- Say you want to order your test methods so as to guarantee a sequence of states for the database
  tags:: Spring, Spring Boot, Junit, Testing
  collapsed:: true
	- ```java
	  
	  @SpringBootTest
	  @ActiveProfiles("integrationTest")
	  @AutoConfigureMockMvc
	  @TestMethodOrder(MethodOrderer.OrderAnnotation.class)
	  public class EmployeeHierarchyFullEnvIT {
	      @Autowired
	      private MockMvc mockMvc;
	  
	      @Autowired
	      private JdbcTemplate jdbcTemplate;
	  
	      @Test
	      @Order(1000)
	      public void firstTest() throws Exception {
	      }
	  
	      @Test
	      @Order(1100)
	      public void secondTest() throws Exception {
	      }
	  ```
- Programmatically bring up the database container and configure Spring context for it (I prefer the `application.properties` way)
  collapsed:: true
	- ```java
	  @RunWith(SpringRunner.class)
	  @SpringBootTest
	  @ContextConfiguration(initializers = {UserRepositoryTCIntegrationTest.Initializer.class})
	  public class UserRepositoryTCIntegrationTest extends UserRepositoryCommonIntegrationTests {
	  
	      @ClassRule
	      public static PostgreSQLContainer postgreSQLContainer = new PostgreSQLContainer("postgres:11.1")
	        .withDatabaseName("integration-tests-db")
	        .withUsername("sa")
	        .withPassword("sa");
	  
	      static class Initializer
	        implements ApplicationContextInitializer<ConfigurableApplicationContext> {
	          public void initialize(ConfigurableApplicationContext configurableApplicationContext) {
	              TestPropertyValues.of(
	                "spring.datasource.url=" + postgreSQLContainer.getJdbcUrl(),
	                "spring.datasource.username=" + postgreSQLContainer.getUsername(),
	                "spring.datasource.password=" + postgreSQLContainer.getPassword()
	              ).applyTo(configurableApplicationContext.getEnvironment());
	          }
	      }
	  }
	  ```
- One Database per Test with Configuration
	-