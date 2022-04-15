- Want access to the EntityManager? #Spring
  collapsed:: true
	- ```java
	  @PersistenceContext
	  final EntityManager entityManager;
	  ```
- Want a pessimistic write lock (select for update) ? #Spring #transaction #[[Spring Transaction]]
  collapsed:: true
	- ```java
	  @Transactional(propagation = Propagation.REQUIRED, timeout = TIMEOUT_SECONDS)
	  @Lock(LockModeType.PESSIMISTIC_WRITE)
	  ```
- Multiple spring and testing things
  collapsed:: true
	- ```java
	  // activating profiles for the execution of the test class
	  @ActiveProfiles({ "nonVerboseDatabaseLogs", "integrationTest" })
	  @DataJpaTest(includeFilters = {
	          @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, value = BalanceService.class),
	    		@ComponentScan.Filter(type = FilterType.REGEX, pattern = "com.example.db.relational.repository.*")
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
  collapsed:: true
	- ```yml
	  # in your spring application.yml (or properties)
	  spring:
	    jpa:
	      properties:
	        hibernate:
	          show_sql: false
	  ```
- Want to clear tables from a database between junit integration tests?
  collapsed:: true
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
  collapsed:: true
	- ```java
	  @DynamicPropertySource
	  static void postgresqlProperties(DynamicPropertyRegistry registry) {
	    registry.add("spring.datasource.url", 
	            () -> String.format("jdbc:postgresql://localhost:%d/prop", postgres.getFirstMappedPort()));
	  }
	  ```
- One of multiple ways of using a Testcontainers database on a spring boot test
  collapsed:: true
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
- Handling exceptions on your spring web application
  collapsed:: true
	- globally
		- ```java
		  @RestControllerAdvice
		  @Slf4j
		  public class GlobalControllerExceptionHandler extends ResponseEntityExceptionHandler {
		    
		      @ExceptionHandler(RuntimeException.class)
		      public ResponseEntity<ErrorResponse> handleGenericRuntimeException(HttpServletRequest req, Exception e) {
		        
		          return new ResponseEntity<>(
		            new YourErrorResponse(/*...*/),
		            HttpStatus.INTERNAL_SERVER_ERROR
		          );
		      }
		  }
		  ```
- Want to listen spring events?
  collapsed:: true
	- ```java
	  @EventListener
	  publi void handleContextRefreshEvent(ContextRefreshedEvent event) {
	  }
	  ```
- Want to create an Aspect?
  collapsed:: true
	- ```java
	  @Aspect
	  @Component  // if you are using Spring AOP
	  public class LoggerAspect {
	      @Around("execution(* com.example.business..*.* (..))")
	      public Object logBeforeAndAfterMethods(ProceedingJoinPoint pjp) throws Throwable { ... }
	  }
	  ```
- Want to change the place where your jpa named queries are defined?
  collapsed:: true
	- ```java
	  @EnableJpaRepositories(namedQueriesLocation = "META-INF(or any other folder, or no folder)/new-name.properties")
	  ```
	- #+BEGIN_NOTE
	  YML file also works (barely), HOWEVER, seems like you cannot use any of YML advantages over properties, so maybe better stick to properties?
	  For example, multiline strings doesn't work, property nesting doesn't work...
	  #+END_NOTE
- Want a simple spring security configuration example?
  collapsed:: true
	- ```java
	  import org.springframework.context.annotation.Bean;
	  import org.springframework.security.config.annotation.authentication.builders.AuthenticationManagerBuilder;
	  import org.springframework.security.config.annotation.web.builders.HttpSecurity;
	  import org.springframework.security.config.annotation.web.builders.WebSecurity;
	  import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
	  import org.springframework.security.config.annotation.web.configuration.WebSecurityConfigurerAdapter;
	  import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
	  import org.springframework.security.crypto.password.PasswordEncoder;
	  
	  @EnableWebSecurity
	  public class SecurityConfig extends WebSecurityConfigurerAdapter {
	  
	      @Override
	      public void configure(WebSecurity web) throws Exception {
	          web.ignoring()
	                  // Spring Security should completely ignore URLs starting with /resources/
	                  .antMatchers("/resources/**");
	      }
	  
	      @Override
	      protected void configure(HttpSecurity http) throws Exception {
	          http
	                  .authorizeRequests()
	                  .antMatchers("/swagger-ui/**", "/v3/api-docs/**")
	                  .permitAll()
	                  .mvcMatchers("/balance/admin/**")
	                  .hasAnyAuthority("ADMIN")
	                  .mvcMatchers("/balance/**")
	                  .hasAnyAuthority("USER", "ADMIN")
	                  .anyRequest()
	                  .authenticated()
	                  .and()
	                  .csrf()
	                  .disable()
	                  .httpBasic();
	      }
	  
	      @Override
	      protected void configure(AuthenticationManagerBuilder auth) throws Exception {
	          auth.inMemoryAuthentication()
	                  .withUser("swagger")
	                  .password(passwordEncoder().encode("swagger"))
	                  .authorities("USER")
	                  .and()
	                  .withUser("transference-svc")
	                  .password(passwordEncoder().encode("transference-svc"))
	                  .authorities("USER")
	                  .and()
	                  .withUser("admin")
	                  .password(passwordEncoder().encode("admin"))
	                  .authorities("USER", "ADMIN");
	      }
	  
	      @Bean
	      public PasswordEncoder passwordEncoder() {
	          return new BCryptPasswordEncoder();
	      }
	  }
	  ```
- Asserting for the throw of an exception 
  collapsed:: true
	- ```java
	  assertThrowsExactly(NotEnoughBalanceException.class, 
	                      () -> balanceService.reserve(reservationCode.toString()));
	  ```
- Best way to compare if a BigXXX (BigDecimal, BigInteger) is equal to another
  collapsed:: true
	- `big.compareTo(another) == 0`
	  collapsed:: true
		- why?
			- because `new BigDecimal("1.00").equals(new BigDecimal("1.0"))` is false.
- Test classes related
	- Pure JUnit + Mockito
		- ```java
		  @ExtendWith(MockitoExtension.class)
		  @MockitoSettings(strictness = Strictness.LENIENT)
		  class UpdateReservationUndoServiceTest {
		      
		      @Mock
		      BalanceRepository balanceRepository;
		      
		      // Mock will have custom name (shown in verification errors)
		      @Mock(name = "database") 
		      ArticleDatabase dbMock;
		      
		      //Instance for spying is created by calling constructor explicitly:
		      @Spy
		      Foo spyOnFoo = new Foo("argument");
		      //Instance for spying is created by mockito via reflection (only default constructors supported):
		      @Spy
		      Bar spyOnBar;
		  
		      @Captor
		      ArgumentCaptor<AsyncCallback<Foo>> captor;
		      
		      @InjectMocks
		      UpdateReservationUndoService service;
		  
		      @Test 
		      public void shouldDoSomethingUseful() {
		          //...
		          verify(mock).doStuff(captor.capture());
		          assertEquals("foo", captor.getValue());
		      }
		  }
		  ```
	- Setting private/final field: `ReflectionTestUtils.setField(balanceUpdateUndoEntity, "id", UUID1);`