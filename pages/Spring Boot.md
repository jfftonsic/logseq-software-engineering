- [Common application.properties list](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html)
  collapsed:: true
	- <iframe src="https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html" height="500"></iframe>
- [[Spring Boot + Liquibase]]
- Testing
	- Testing spring data repositories, use @ [[DataJpaTest]]
		- Sample source:
		  id:: 623262f9-f6d5-4383-a7de-f9f0d7574a8a
			- id:: 62326304-d575-4c2d-bd1c-1184412cb068
			  ```java
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
			-