tags:: Frameworks

-
- [[Spring Boot]]
- [[Spring Transaction]]
- [[Spring Data]]
- [[Spring Web]]
- [[Spring Integration]]
- [[Spring Localization]]
-
- Tips and tricks
  heading:: true
	- Building URI/URL 's
	  collapsed:: true
		- ```java
		          UriComponentsBuilder.newInstance()
		                  .scheme(URIScheme.HTTP.getId())
		                  .host("host")
		                  .port(8000)
		                  .path("/employee")
		                  .pathSegment("{employeeName}", "{anotherThing}")
		                  // this build(xxx) will automatically perform encoding
		                  .build("John", "YetMoreData")
		                  .toURL();
		  ```
	- Building `HttpHeaders` from a `HttpServletRequest`
	  collapsed:: true
		- ```java
		  // ...
		  import org.springframework.data.util.Pair;
		  import org.springframework.http.HttpHeaders;
		  import javax.servlet.http.HttpServletRequest;
		  //...
		  	public HttpHeaders getHttpHeaders(HttpServletRequest request) {
		          final var httpHeaders = new HttpHeaders();
		  
		          Collections.list(request.getHeaderNames()).stream()
		                  .mapMulti(
		                          (String headerKey, Consumer<Pair<String, String>> consumer) ->
		                                  Collections.list(request.getHeaders(headerKey))
		                                          .forEach(
		                                                  value -> consumer.accept(Pair.of(headerKey, value))
		                                          )
		                  ).forEach(pair -> httpHeaders.add(pair.getFirst(), pair.getSecond()));
		  
		          return httpHeaders;
		      }
		  ```
	- A `mockMvc` test example
	  collapsed:: true
		- ```java
		          mockMvc.perform(
		                          post(
		                                  UriComponentsBuilder.newInstance()
		                                          .path(EMPLOYEE_SVC_URI)
		                                          .pathSegment("{employeeName}")
		                                          .build("John")
		                          )
		                                  .with(
		                                          SecurityMockMvcRequestPostProcessors
		                                                  .user(VALID_ADMIN_USER)
		                                                  .password(VALID_ADMIN_PASSWORD)
		                                                  .authorities(new SimpleGrantedAuthority(ROLE_ADMIN))
		                                  )
		                  )
		                  .andDo(print())
		                  .andExpect(status().isOk())
		                  .andExpect(content().contentType(MediaType.APPLICATION_JSON));
		  ```
- Spring fast documentation links
  collapsed:: true
  #WIP
	- [[Spring Boot]]
		- [Everything in single html](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle)
		- [Index of the documentation in separate htmls](https://docs.spring.io/spring-boot/docs/current/reference/html/index.html)
		- [Application properties reference](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties)
		- Build Systems
			- [Gradle](https://docs.spring.io/spring-boot/docs/current/reference/html/using.html#using.build-systems.gradle)
			- [Starters](https://docs.spring.io/spring-boot/docs/current/reference/html/using.html#using.build-systems.starters)
		- Web
			- [Spring MVC, Jersey, Embedded Servlet Containers](https://docs.spring.io/spring-boot/docs/current/reference/html/web.html#web.servlet)
			- [Graceful Shutdown](https://docs.spring.io/spring-boot/docs/current/reference/html/web.html#web.graceful-shutdown)
			- [Default Security Configuration, Auto-configuration for OAuth2, SAML](https://docs.spring.io/spring-boot/docs/current/reference/html/web.html#web.security)
		- Data
			- [[SQL]]
				- [Configuring a SQL Datastore, Embedded Database support, Connection pools, and more.](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql)
			- [[NOSQL]]
				- [Auto-configuration for NOSQL stores such as Redis, MongoDB, Neo4j, and others.](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql)
			- *   [1.1. Configure a DataSource](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.datasource)
			          *   [1.1.1. Embedded Database Support](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.datasource.embedded)
			          *   [1.1.2. Connection to a Production Database](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.datasource.production)
			          *   [1.1.3. DataSource Configuration](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.datasource.configuration)
			          *   [1.1.4. Supported Connection Pools](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.datasource.connection-pool)
			          *   [1.1.5. Connection to a JNDI DataSource](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.datasource.jndi)
			      *   [1.2. Using JdbcTemplate](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jdbc-template)
			      *   [1.3. JPA and Spring Data JPA](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jpa-and-spring-data)
			          *   [1.3.1. Entity Classes](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jpa-and-spring-data.entity-classes)
			          *   [1.3.2. Spring Data JPA Repositories](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jpa-and-spring-data.repositories)
			          *   [1.3.3. Spring Data Envers Repositories](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jpa-and-spring-data.envers-repositories)
			          *   [1.3.4. Creating and Dropping JPA Databases](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jpa-and-spring-data.creating-and-dropping)
			          *   [1.3.5. Open EntityManager in View](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jpa-and-spring-data.open-entity-manager-in-view)
			      *   [1.4. Spring Data JDBC](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jdbc)
			      *   [1.5. Using H2???s Web Console](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.h2-web-console)
			          *   [1.5.1. Changing the H2 Console???s Path](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.h2-web-console.custom-path)
			      *   [1.6. Using jOOQ](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jooq)
			          *   [1.6.1. Code Generation](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jooq.codegen)
			          *   [1.6.2. Using DSLContext](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jooq.dslcontext)
			          *   [1.6.3. jOOQ SQL Dialect](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jooq.sqldialect)
			          *   [1.6.4. Customizing jOOQ](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.jooq.customizing)
			      *   [1.7. Using R2DBC](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.r2dbc)
			          *   [1.7.1. Embedded Database Support](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.r2dbc.embedded)
			          *   [1.7.2. Using DatabaseClient](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.r2dbc.using-database-client)
			          *   [1.7.3. Spring Data R2DBC Repositories](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.sql.r2dbc.repositories)
			  *   [2\. Working with NoSQL Technologies](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql)
			      *   [2.1. Redis](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.redis)
			          *   [2.1.1. Connecting to Redis](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.redis.connecting)
			      *   [2.2. MongoDB](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.mongodb)
			          *   [2.2.1. Connecting to a MongoDB Database](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.mongodb.connecting)
			          *   [2.2.2. MongoTemplate](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.mongodb.template)
			          *   [2.2.3. Spring Data MongoDB Repositories](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.mongodb.repositories)
			          *   [2.2.4. Embedded Mongo](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.mongodb.embedded)
			      *   [2.3. Neo4j](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.neo4j)
			          *   [2.3.1. Connecting to a Neo4j Database](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.neo4j.connecting)
			          *   [2.3.2. Spring Data Neo4j Repositories](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.neo4j.repositories)
			      *   [2.4. Solr](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.solr)
			          *   [2.4.1. Connecting to Solr](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.solr.connecting)
			      *   [2.5. Elasticsearch](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.elasticsearch)
			          *   [2.5.1. Connecting to Elasticsearch using REST clients](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.elasticsearch.connecting-using-rest)
			              *   [Connecting to Elasticsearch using RestHighLevelClient](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.elasticsearch.connecting-using-rest.restclient)
			              *   [Connecting to Elasticsearch using ReactiveElasticsearchClient](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.elasticsearch.connecting-using-rest.webclient)
			          *   [2.5.2. Connecting to Elasticsearch by Using Spring Data](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.elasticsearch.connecting-using-spring-data)
			          *   [2.5.3. Spring Data Elasticsearch Repositories](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.elasticsearch.repositories)
			      *   [2.6. Cassandra](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.cassandra)
			          *   [2.6.1. Connecting to Cassandra](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.cassandra.connecting)
			          *   [2.6.2. Spring Data Cassandra Repositories](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.cassandra.repositories)
			      *   [2.7. Couchbase](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.couchbase)
			          *   [2.7.1. Connecting to Couchbase](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.couchbase.connecting)
			          *   [2.7.2. Spring Data Couchbase Repositories](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.couchbase.repositories)
			      *   [2.8. LDAP](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.ldap)
			          *   [2.8.1. Connecting to an LDAP Server](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.ldap.connecting)
			          *   [2.8.2. Spring Data LDAP Repositories](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.ldap.repositories)
			          *   [2.8.3. Embedded In-memory LDAP Server](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.ldap.embedded)
			      *   [2.9. InfluxDB](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.influxdb)
			          *   [2.9.1. Connecting to InfluxDB](https://docs.spring.io/spring-boot/docs/current/reference/html/data.html#data.nosql.influxdb.connecting)
		- Messaging
			- [[AMQP]]
				- [Auto-configuration for RabbitMQ](https://docs.spring.io/spring-boot/docs/current/reference/html/messaging.html#messaging.amqp)
			- [[Kafka]]
				- [Auto-configuration for Spring Kafka](https://docs.spring.io/spring-boot/docs/current/reference/html/messaging.html#messaging.kafka)
			- [[Spring Integration]]
				- [Auto-configuration for Spring Integration](https://docs.spring.io/spring-boot/docs/current/reference/html/messaging.html#messaging.spring-integration)
		- IO
			- [[Caching]]
				- [Caching support EhCache, Hazelcast, Infinispan and more](https://docs.spring.io/spring-boot/docs/current/reference/html/io.html#io.caching)
			- Validation
				- [JSR-303 Validation](https://docs.spring.io/spring-boot/docs/current/reference/html/io.html#io.validation)
			- [[REST]] Clients
				- [Calling REST Services with RestTemplate and WebClient](https://docs.spring.io/spring-boot/docs/current/reference/html/io.html#io.rest-client)
			- [[JTA]]
				- [Distributed Transactions with JTA](https://docs.spring.io/spring-boot/docs/current/reference/html/io.html#io.jta)
		- Container images
			- **Efficient Container Images:** [Tips to optimize container images such as Docker images](container-images.html#container-images.efficient-images)
			- **Dockerfiles:** [Building container images using dockerfiles](container-images.html#container-images.dockerfiles)
			- **Cloud Native Buildpacks:** [Support for Cloud Native Buildpacks with Maven and Gradle](container-images.html#container-images.buildpacks)
		- Deployment
			- [Cloud Deployment](deployment.html#deployment.cloud)
			- [OS Service](deployment.html#deployment.installing.nix-services)
- Bean self reference
  collapsed:: true
	- Sometimes you need to run a method from the bean instance itself but also taking into account the java assist or wrapping that spring did on your bean.
	  
	  For example, say your class has a `@Transactional(required)` method called `m1` which needs to call a method `m2` that has a `@Transactional(requires_new)` on the same class.
	  If you want that to work as expected, you cannot just do a `this.m2(...)` on `m1`, you need to do as the example below.
		- ```java
		  @Service
		  @RequiredArgsConstructor(onConstructor = @__(@Autowired))
		  @FieldDefaults(level = AccessLevel.PRIVATE)
		  @Slf4j
		  public class JustAService {
		    	final ApplicationContext context;
		    	JustAService self;
		    
		      public JustAService getSelf() {
		          if (self == null)
		              self = context.getBean(JustAService.class);
		          return self;
		      }
		    
		      @Transactional
		      public void thisMethodDoesNotEvenNeedToBeAMethodThatRequiresClassWrapping() {
		  
		              getSelf().aMethodThatRequiresClassWrappingOfSomeSort();
		  //          aMethodThatRequiresClassWrappingOfSomeSort(idemCode, idemActor, requestTimestamp, amount); // this doesn't create the new transaction for the call
		          
		      }
		    
		      @Transactional(propagation = Propagation.REQUIRES_NEW)
		      public void aMethodThatRequiresClassWrappingOfSomeSort() {
		        //...
		      }
		        
		  }
		  
		  ```
- Changing default bean initialization laziness
  sources:: [Lazy Initialization in Spring Boot 2.2](https://spring.io/blog/2019/03/14/lazy-initialization-in-spring-boot-2-2)
	- `spring.main.lazy-initialization` property
-
- [Shutting Down the Spring IoC Container Gracefully in Non-Web Applications](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-factory-shutdown)
  collapsed:: true
	- **not for web-based ApplicationContex**
	- register a shutdown hook with the JVM
	- ```java
	      public static void main(final String[] args) throws Exception {
	          ConfigurableApplicationContext ctx = new ClassPathXmlApplicationContext("beans.xml");
	  
	          // add a shutdown hook for the above context...
	          ctx.registerShutdownHook();
	  
	          // app runs here...
	  
	          // main method exits, hook is called prior to the app shutting down...
	      }
	  ```
-
- In [[Spring Boot]] testing how to set a context property dynamically?
  sources:: https://www.baeldung.com/spring-dynamicpropertysource, https://stackoverflow.com/questions/67413169/spring-data-jdbc-testcontainers-datasource
  collapsed:: true
	- One use case for example:
	  collapsed:: true
		- You are bringing up a database [[Testcontainer]] and want to set its varying configuration on the spring context
		- ```java
		  @SpringBootTest
		  @Testcontainers
		  public class AccountRepositoryTest {
		  
		      @Container
		      public static PostgreSQLContainer<?> postgresqlContainer = new PostgreSQLContainer<>();
		  
		    //...
		    
		      @DynamicPropertySource
		      static void postgresqlProperties(DynamicPropertyRegistry registry) {
		          registry.add("spring.datasource.url", postgresqlContainer::getJdbcUrl);
		          registry.add("spring.datasource.username", postgresqlContainer::getUsername);
		          registry.add("spring.datasource.password", postgresqlContainer::getPassword);
		      }
		  }
		  ```
		  #sample #testcontainers #Database #testing #[[Spring Boot]] #junit
	- Is there another way?
	  collapsed:: true
		- ```java
		  @SpringBootTest
		  @Testcontainers
		  @ContextConfiguration(initializers = ArticleTraditionalLiveTest.EnvInitializer.class)
		  class ArticleTraditionalLiveTest {
		  
		      @Container
		      static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:11")
		        .withDatabaseName("prop")
		        .withUsername("postgres")
		        .withPassword("pass")
		        .withExposedPorts(5432);
		  
		    	static class EnvInitializer implements ApplicationContextInitializer<ConfigurableApplicationContext> {
		  
		          @Override
		          public void initialize(ConfigurableApplicationContext applicationContext) {
		              TestPropertyValues.of(
		                String.format("spring.datasource.url=jdbc:postgresql://localhost:%d/prop", postgres.getFirstMappedPort()),
		                "spring.datasource.username=postgres",
		                "spring.datasource.password=pass"
		              ).applyTo(applicationContext);
		          }
		      }
		  
		      // omitted 
		  }
		  ```
		  #sample #testcontainers #Database #testing #[[Spring Boot]] #junit
	- Is there even another way?
	  collapsed:: true
		- Using junit 5 test fixtures
			- ```java
			  public class PostgreSQLExtension implements BeforeAllCallback, AfterAllCallback {
			  
			      private PostgreSQLContainer<?> postgres;
			  
			      @Override
			      public void beforeAll(ExtensionContext context) {
			          postgres = new PostgreSQLContainer<>("postgres:11")
			            .withDatabaseName("prop")
			            .withUsername("postgres")
			            .withPassword("pass")
			            .withExposedPorts(5432);
			  
			          postgres.start();
			          String jdbcUrl = String.format("jdbc:postgresql://localhost:%d/prop", postgres.getFirstMappedPort());
			          System.setProperty("spring.datasource.url", jdbcUrl);
			          System.setProperty("spring.datasource.username", "postgres");
			          System.setProperty("spring.datasource.password", "pass");
			      }
			  
			      @Override
			      public void afterAll(ExtensionContext context) {
			          // do nothing, Testcontainers handles container shutdown
			      }
			  }
			  
			  @SpringBootTest
			  @ExtendWith(PostgreSQLExtension.class)
			  @DirtiesContext // recreates the application context and allows our test classes to interact with a separate PostgreSQL instance, running on a random port
			  public class ArticleTestFixtureLiveTest {
			      // just the test code
			  }
			  ```
			  Dfasdf
-
- Spring context events
	- sources:: [Baeldung Spring Events](https://www.baeldung.com/spring-events), [Baeldung Spring Application Context Events](https://www.baeldung.com/spring-context-events)
	- Spring has an event mechanism which is built around the `ApplicationContext`. It can be used to exchange information between different beans. And it also serves the purpose of listening to Spring context's own events.
		- Custom events
			- an event class
			  collapsed:: true
				- ```java
				  public class CustomSpringEvent extends ApplicationEvent {
				      private String message;
				  
				      public CustomSpringEvent(Object source, String message) {
				          super(source);
				          this.message = message;
				      }
				      public String getMessage() {
				          return message;
				      }
				  }
				  ```
			- a publisher
			  collapsed:: true
				- ```java
				  @Component
				  public class CustomSpringEventPublisher {
				      @Autowired
				      private ApplicationEventPublisher applicationEventPublisher;
				  
				      public void publishCustomEvent(final String message) {
				          System.out.println("Publishing custom event. ");
				          CustomSpringEvent customSpringEvent = new CustomSpringEvent(this, message);
				          applicationEventPublisher.publishEvent(customSpringEvent);
				      }
				  }
				  ```
			- publisher for generic typed events
			  collapsed:: true
				- Due to type erasure, we need to publish an event that resolves the generics parameter we would filter on, for example, `class GenericStringSpringEvent extends GenericSpringEvent<String>`.
			- publishing from a listener method
			  collapsed:: true
				- return a non-null value from a method annotated with @EventListener
			- publish multiple new events
			  collapsed:: true
				- returning them in a collection as the result of event processing.
			- listener  using annotation (preferred)
			  collapsed:: true
				- A `@EventListener` method can return a non-void type.
					- If the value returned is non-null, the event mechanism will publish a new event for it.
				- ```java
				  @Component
				  public class AnnotationDrivenEventListener {
				      @EventListener
				      public void handleContextStart(ContextStartedEvent cse) {
				          System.out.println("Handling context started event.");
				      }
				    
				      @EventListener(classes = { ContextStartedEvent.class, ContextStoppedEvent.class })
				      public void handleMultipleEvents() {
				          System.out.println("Multi-event listener invoked");
				      }
				  }
				  ```
			- listener  using interface
			  collapsed:: true
				- ```java
				  @Component
				  public class CustomSpringEventListener implements ApplicationListener<CustomSpringEvent> {
				      @Override
				      public void onApplicationEvent(CustomSpringEvent event) {
				          System.out.println("Received spring custom event - " + event.getMessage());
				      }
				  }
				  ```
			- listening to multiple events
			  collapsed:: true
				- ```java
				  @EventListener(classes = { ContextStartedEvent.class, ContextStoppedEvent.class })
				  public void handleMultipleEvents() {
				      System.out.println("Multi-event listener invoked");
				  }
				  ```
			- listener  using generics
			  collapsed:: true
				- ```java
				  public class GenericSpringEvent<T> {
				      private T what;
				      protected boolean success;
				  
				      public GenericSpringEvent(T what, boolean success) {
				          this.what = what;
				          this.success = success;
				      }
				      // ... standard getters
				  }
				  
				  @Component
				  public class GenericSpringEventListener 
				    implements ApplicationListener<GenericSpringEvent<String>> {
				      @Override
				      public void onApplicationEvent(@NonNull GenericSpringEvent<String> event) {
				          System.out.println("Received spring generic event - " + event.getWhat());
				      }
				  }
				  ```
			- conditional listener
			  collapsed:: true
				- ```java
				  @Component
				  public class AnnotationDrivenEventListener {
				      @EventListener(condition = "#event.success")
				      public void handleSuccessful(ContextStartedEvent event) {
				          System.out.println("Handling event (conditional).");
				      }
				  }
				  ```
			- listening transaction-bound events
			  collapsed:: true
				- allows binding the listener of an event to a phase of the database transaction.
				- AFTER_COMMIT (default) is used to fire the event if the transaction has completed successfully.
				  AFTER_ROLLBACK ??? if the transaction has rolled back
				  AFTER_COMPLETION ??? if the transaction has completed (an alias for AFTER_COMMIT and AFTER_ROLLBACK)
				  BEFORE_COMMIT is used to fire the event right before transaction commit.
				- **listener will be invoked only if there's a transaction in which the event producer is running**
					- if **no transaction** is running, the **event isn???t sent** at all **unless** we override this by setting `fallbackExecution` attribute to true.
				- ```java
				  @TransactionalEventListener(phase = TransactionPhase.BEFORE_COMMIT)
				  public void handleCustom(CustomSpringEvent event) {
				      System.out.println("Handling event inside a transaction BEFORE COMMIT.");
				  }
				  ```
		- asynchronous events
		  collapsed:: true
			- The event, the publisher and the listener implementations **remain the same** as before, but now the listener will asynchronously deal with the event in a separate thread.
			- ```java
			  @Configuration
			  public class AsynchronousSpringEventsConfig {
			      @Bean(name = "applicationEventMulticaster")
			      public ApplicationEventMulticaster simpleApplicationEventMulticaster() {
			          SimpleApplicationEventMulticaster eventMulticaster =
			            new SimpleApplicationEventMulticaster();
			          
			          eventMulticaster.setTaskExecutor(new SimpleAsyncTaskExecutor());
			          return eventMulticaster;
			      }
			  }
			  ```
		- Spring events
			- hook into the lifecycle of an application and the context
			- See the built-in events here, [in the documentation](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#context-functionality-events)