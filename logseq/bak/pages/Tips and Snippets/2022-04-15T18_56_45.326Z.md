- Want access to the EntityManager? #Spring
	- ```java
	  @PersistenceContext
	  final EntityManager entityManager;
	  ```
- Want a pessimistic write lock (select for update) ? #Spring #transaction #[[Spring Transaction]]
	- ```java
	  @Transactional(propagation = Propagation.REQUIRED, timeout = TIMEOUT_SECONDS)
	  @Lock(LockModeType.PESSIMISTIC_WRITE)
	  ```
- Multiple spring and testing things
	- ```java
	  // activating profiles for the execution of the test class
	  @ActiveProfiles({ "nonVerboseDatabaseLogs", "integrationTest" })
	  @DataJpaTest(includeFilters = {
	          @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, value = BalanceService.class),
	  })
	  //
	  @AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
	  // importing other configurations
	  @Import(FakeClockConfiguration.class)
	  // if you wanna set some properties for the execution of this class
	  @TestPropertySource(properties = {
	          "spring.datasource.hikari.auto-commit=false",
	          "logging.level.org.hibernate.resource.jdbc.internal=INFO",
	    		//...
	  })
	  class BalanceServiceWithDatabaseContextTest {
	  }
	  ```
- How to disable hibernate logging the HQL on the stdout?
	- ```yml
	  # in your spring application.yml (or properties)
	  spring:
	    jpa:
	      properties:
	        hibernate:
	          show_sql: false
	  ```
- Want to clear tables from a database between junit integration tests?
	- You can do with `@DirtiesContext(classMode = DirtiesContext.ClassMode.AFTER_EACH_TEST_METHOD)` but this will reload spring context each method... so look below
	- Julian's answer to [this stackoverflow question](https://stackoverflow.com/questions/48714118/reset-database-after-each-test-on-spring-without-using-dirtiescontext)
	- You can clean the tables you need by doing the following:
	- 1.  Inject a JdbcTemplate instance
	- ```java
	  @Autowired
	  private JdbcTemplate jdbcTemplate;
	  ```
	- 2.  Use the class JdbcTestUtils to delete the records from the tables you need to.
	- `JdbcTestUtils.deleteFromTables(jdbcTemplate, "table1", "table2", "table3");`
	- 3.  Call this line in the method annotated with `@After` or `@AfterEach` in your test class:
	- ```java
	  @AfterEach
	    void tearDown() throws DatabaseException {
	        JdbcTestUtils.deleteFromTables(jdbcTemplate, "table1", "table2", "table3");
	    }
	  ```
	- blog post: [Easy Integration Testing With Testcontainers](https://mydeveloperplanet.com/2020/05/05/easy-integration-testing-with-testcontainers/)
- Setting dynamic properties
	- ```java
	  @DynamicPropertySource
	  static void postgresqlProperties(DynamicPropertyRegistry registry) {
	    registry.add("spring.datasource.url", 
	            () -> String.format("jdbc:postgresql://localhost:%d/prop", postgres.getFirstMappedPort()));
	  }
	  ```
- One of multiple ways of using a Testcontainers database on a spring boot test
	- ```java
	  @SpringBootTest
	  @Testcontainers
	  public class ArticleLiveTest {
	  
	      @Container
	      static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:11")
	        .withDatabaseName("prop")
	        .withUsername("postgres")
	        .withPassword("pass")
	        .withExposedPorts(5432);
	  
	      @DynamicPropertySource
	      static void registerPgProperties(DynamicPropertyRegistry registry) {
	          registry.add("spring.datasource.url", 
	            () -> String.format("jdbc:postgresql://localhost:%d/prop", postgres.getFirstMappedPort()));
	          registry.add("spring.datasource.username", () -> "postgres");
	          registry.add("spring.datasource.password", () -> "pass");
	      }
	      
	      // tests are same as before
	  }
	  ```
- Asserting for the throw of an exception
	- ```java
	  assertThrowsExactly(NotEnoughBalanceException.class, 
	                      () -> balanceService.reserve(reservationCode.toString()));
	  ```
- Best way to compare if a BigXXX (BigDecimal, BigInteger) is equal to another
	- `big.compareTo(another) == 0`
	  collapsed:: true
		- why?
			- because `new BigDecimal("1.00").equals(new BigDecimal("1.0"))` is false.
-