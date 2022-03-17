sources:: https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/autoconfigure/orm/jpa/DataJpaTest.html

- Annotation for a JPA test that focuses only on JPA components.
- By default
	- tests annotated with @DataJpaTest are **transactional and roll back at the end of each test**.
	- **use an embedded in-memory database (replacing any explicit or usually auto-configured DataSource)**.
		- The `@AutoConfigureTestDatabase` annotation can be used to override these settings.
- SQL queries are **logged by default by setting the spring.jpa.show-sql property to true**. This can be disabled using the showSql attribute.
- #+BEGIN_TIP
  If you are looking to load your full application configuration, but use an embedded database, you should consider @[[SpringBootTest]] combined with @AutoConfigureTestDatabase rather than this annotation.
  #+END_TIP