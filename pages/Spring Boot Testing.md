alias:: Spring Boot Test

- The Spring test process fires up **one or more instances of the Spring container** to run tests on. If it thinks the config is exactly the same for two tests it will **re-use an instance**, otherwise it will start a new one.
	- The instances are shared to avoid needing to start a whole new [[Spring Boot]] application for each test.
	- The problem is that the **instances may share some resources, such as databases and network ports**. Therefore _errors might occur from trying to start multiple instances at the same time_.
- Spring Boot MVC Tests
	- `@WebMvcTest`: to test controllers 
	  sources:: [Auto-configured Spring MVC Tests](https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.testing.spring-boot-applications.spring-mvc-tests)
		- limits scanned beans to `@Controller, @ControllerAdvice, @JsonComponent, Converter, GenericConverter, Filter, HandlerInterceptor, WebMvcConfigurer, WebMvcRegistrations, and HandlerMethodArgumentResolver`. Regular `@Component` and `@ConfigurationProperties` beans are not scanned when the @WebMvcTest annotation is used.
		- explained sample: (it also uses [[Spring Security]])
			- ```java
			  
			  // I'm using this to replace a injected bean on the controller, see @Import
			  @Configuration
			  class ExtraConf {
			  
			    	// PS: the name of this method cannot be the same as the method on the
			    	// real configuration file.
			      @Bean
			      @Primary
			      public Clock testClock() {
			          return Clock.fixed(Instant.EPOCH, ZoneId.of("UTC"));
			      }
			  }
			  
			  @WebMvcTest
			  @Import(ExtraConf.class)
			  class BalanceControllerTest {
			  
			      @Autowired
			      private MockMvc mockMvc;
			  
			      // this injects a mock incide the controller.
			      @MockBean
			      IBalanceService service;
			  
			      @Test
			      void getBalance() throws Exception {
			          Mockito.when(service.fetchAmount()).thenReturn(BigDecimal.ZERO);
			          
			          mockMvc.perform(
			                          get("/balance")
			                                  .with(csrf())
			                                  .with(
			                                          SecurityMockMvcRequestPostProcessors
			                                                  .user("swagger")
			                                                  .authorities(new SimpleGrantedAuthority("USER"))
			                                  )
			                  )
			                  .andDo(print())
			                  .andExpect(
			                          status().isOk()
			                  )
			                  .andExpect(
			                          jsonPath("$.amount")
			                          .value("0")
			                  );
			      }
			  }
			  ```
- Samples
	- @[[SpringBootTest]] + @ [[Testcontainers]]
	  collapsed:: true
		- from https://github.com/wearearima/spring-data-jdbc-testcontainers/blob/master/src/test/java/eu/arima/springdatajdbctestcontainers/AccountRepositoryTest.java at [[Mar 17th, 2022]]
			- ```java
			  import java.util.Arrays;
			  import java.util.Date;
			  
			  import org.junit.Assert;
			  import org.junit.jupiter.api.Disabled;
			  import org.junit.jupiter.api.Test;
			  import org.springframework.beans.factory.annotation.Autowired;
			  import org.springframework.boot.test.context.SpringBootTest;
			  import org.springframework.data.domain.Page;
			  import org.springframework.data.domain.PageRequest;
			  import org.springframework.data.domain.Pageable;
			  import org.springframework.jdbc.core.JdbcTemplate;
			  import org.springframework.jdbc.core.namedparam.NamedParameterJdbcTemplate;
			  import org.springframework.test.context.DynamicPropertyRegistry;
			  import org.springframework.test.context.DynamicPropertySource;
			  import org.springframework.test.jdbc.JdbcTestUtils;
			  import org.springframework.transaction.annotation.Transactional;
			  import org.testcontainers.containers.PostgreSQLContainer;
			  import org.testcontainers.junit.jupiter.Container;
			  import org.testcontainers.junit.jupiter.Testcontainers;
			  
			  
			  @Transactional
			  @SpringBootTest
			  @Testcontainers
			  public class AccountRepositoryTest {
			  
			      @Container
			      public static PostgreSQLContainer<?> postgresqlContainer = new PostgreSQLContainer<>();
			  
			      @Autowired
			      private AccountRepository accountRepository;
			  
			      @Autowired
			      private NamedParameterJdbcTemplate template;
			  
			      @Test
			      public void findById() {
			          Account account = this.accountRepository.findById(1).get();
			  
			          Assert.assertEquals("user1", account.getUsername());
			          Assert.assertEquals("user1@company.com", account.getEmail());
			          System.out.println(account.getCreated());
			      }
			  
			      @Test
			      public void save() {
			          Account account = new Account();
			          account.setName("new");
			          account.setUsername("newusername");
			          account.setEmail("newemail@company.com");
			          account.setCreated(new Date());
			  
			          Account newAccount = this.accountRepository.save(account);
			  
			          Assert.assertEquals(1, JdbcTestUtils.countRowsInTableWhere(
			                  (JdbcTemplate) template.getJdbcOperations(), "account", "id = " + newAccount.getId()
			          ));
			      }
			  
			      @Disabled("Paging repositories not supported https://jira.spring.io/browse/DATAJDBC-101")
			      @Test
			      public void pageable() {
			          Pageable pageable = PageRequest.of(0, 20);
			          Page<Account> accounts = this.accountRepository.findAll(pageable);
			          Assert.assertEquals(3, accounts.getSize());
			      }
			  
			      @Test
			      public void length() {
			          int length = this.accountRepository.accountsLength();
			  
			          Assert.assertEquals(3, length);
			      }
			  
			      @Test
			      public void updateName() {
			          boolean updated = this.accountRepository.updateName(3, "updatedname");
			          Assert.assertTrue(updated);
			      }
			  
			      @Test
			      public void deleteList() {
			          this.accountRepository.deleteIdList(Arrays.asList(1, 2));
			  
			          int length = this.accountRepository.accountsLength();
			  
			          Assert.assertEquals(1, length);
			      }
			      
			      @DynamicPropertySource
			      static void postgresqlProperties(DynamicPropertyRegistry registry) {
			          registry.add("spring.datasource.url", postgresqlContainer::getJdbcUrl);
			          registry.add("spring.datasource.username", postgresqlContainer::getUsername);
			          registry.add("spring.datasource.password", postgresqlContainer::getPassword);
			      }
			  }
			  ```
- Testing spring data repositories, use @ [[DataJpaTest]]
	- Sample source:
	  collapsed:: true
		- ```java
		  @ActiveProfiles("test")
		  @DataJpaTest
		  @AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
		  public class RepositoryTest {
		      @Autowired
		      private BalanceRepository repository;
		  
		      @Test
		      void findAll() {
		          var owners = repository.findAll();
		          final var balanceEntities = new LinkedList<BalanceEntity>();
		          owners.iterator().forEachRemaining(balanceEntities::add);
		          assertThat(balanceEntities.size()).isOne();
		          assertThat(balanceEntities.get(0).getTotalAmount()).isEqualTo(new BigDecimal("0.0"));
		      }
		  }
		  ```
- Using [[Testcontainers]] to bring a database up to test with spring boot
	- application-test.yml
	  collapsed:: true
		- ```yml
		  server:
		    friendlyName: Test Server
		  
		  spring:
		    datasource:
		      driver-class-name: org.testcontainers.jdbc.ContainerDatabaseDriver
		      url: jdbc:tc:postgresql:///
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
		  
		  logging:
		    level:
		      liquibase: INFO
		      org:
		        testcontainers: INFO
		      com:
		        github:
		          dockerjava: WARN
		  ```
		- Then you can use this sample junit class: [[62326304-d575-4c2d-bd1c-1184412cb068]]