sources:: https://docs.liquibase.com/tools-integrations/springboot/springboot.html

- [[Liquibase]] supports a variety of [Liquibase Commands](https://docs.liquibase.com/commands/home.html). Spring Boot may only integrates with a subset of those commands: `update, futureRollbackSQL, dropAll, updateTestingRollback`, and `clearChecksums` commands.
- configuration properties table (at [[Mar 16th, 2022]] )
  collapsed:: true
	- the most updated table can be seen [here](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties.data-migration)
	- | Spring Boot | Liquibase | Description |
	  | spring.liquibase.change-log | changeLogFile | changelog configuration path |
	  | spring.liquibase.labels | labels | Comma-separated list of runtime labels to use |
	  | spring.liquibase.contexts | Contexts | Comma-separated list of runtime contexts to use |
	  | spring.liquibase.database-change-log-lock-table | databaseChangeLogLockTableName | Name of table to use for tracking concurrent Liquibase usage |
	  | spring.liquibase.database-change-log-table | databaseChangeLogTableName | Name of table to use for tracking change history |
	  | spring.liquibase.default-schema | defaultSchemaName | Default database schema |
	  | spring.liquibase.liquibase-schema | liquibaseSchemaName | Schema to use for Liquibase objects |
	  | spring.liquibase.liquibase-tablespace | databaseChangeLogTablespaceName | Tablespace to use for Liquibase objects |
	  | spring.liquibase.parameters.* | parameter.* | changelog parameters |
	  | spring.liquibase.password | password | Login password of the database to migrate |
	  | spring.liquibase.tag |  | Tag name to use when applying database changes. Can also be used with rollbackFile to generate a rollback script for all existing changes associated with that tag |
	  | spring.liquibase.url | url | JDBC URL of the database to migrate. If not set, the primary configured data source is used |
	  | spring.liquibase.user | username | Login user of the database to migrate |
- [[Testing]]
	- id:: 62324847-9550-42c9-a980-b89660b2ced1
	  > By default, tests annotated with @[[DataJpaTest]] will use an embedded in-memory database (replacing any explicit or usually auto-configured DataSource). The @AutoConfigureTestDatabase annotation can be used to override these settings.
	- telling spring test to not replace the datasource you configure by
	  `spring.test.database.replace=none`
	- examples:
		- ```java
		     @RunWith(SpringRunner.class)
		     @ContextConfiguration(classes = PersistenceTestConfig.class)
		     @DataJpaTest
		     @AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
		     @Sql(executionPhase = Sql.ExecutionPhase.BEFORE_TEST_METHOD, scripts = {"classpath:init.sql"})
		     public class MyRepoTest {
		         ...
		      }
		  ```