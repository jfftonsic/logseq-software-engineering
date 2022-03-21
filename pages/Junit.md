- Using [[Test Fixtures]] to initialize a [[PostgreSQL]] [[Testcontainer]] and prepare system properties for a follow-up [[Spring Boot Test]]
	- code:
	  collapsed:: true
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
		  @DirtiesContext
		  public class ArticleTestFixtureLiveTest {
		      // just the test code
		  }
		  ```
		  #sample #java #junit #spring #testcontainers
- Asserts for JSON payloads:
  sources:: https://www.baeldung.com/jsonassert, http://jsonassert.skyscreamer.org/
  collapsed:: true
	- org.skyscreamer:jsonassert:1.5.0
		- check the current version: https://mvnrepository.com/artifact/org.skyscreamer/jsonassert
	- samples:
		- ```java
		  JSONObject data = getRESTData("/friends/367.json");
		  String expected = "{friends:[{id:123,name:\"Corby Page\"},{id:456,name:\"Carter Page\"}]}";
		  JSONAssert.assertEquals(expected, data, false);
		  ```
	- error messages:
		- ```java
		  String expected = "{id:1,name:\"Joe\",friends:[{id:2,name:\"Pat\",pets:[\"dog\"]},{id:3,name:\"Sue\",pets:[\"bird\",\"fish\"]}],pets:[]}";
		  String actual = "{id:1,name:\"Joe\",friends:[{id:2,name:\"Pat\",pets:[\"dog\"]},{id:3,name:\"Sue\",pets:[\"cat\",\"fish\"]}],pets:[]}"
		  JSONAssert.assertEquals(expected, actual, false);
		  ```
		  generates:
		  ```text
		  friends[id=3].pets[]: Expected bird, but not found ; friends[id=3].pets[]: Contains cat, but not expected
		  ```
	- ordering of elements does not matter while dealing with JSON objects
	- the order of elements in an array has to be exactly same in STRICT comparison mode
	- LENIENT Mode: even if the actual JSON contains extended fields, the test will still pass:
		- ```java
		  String actual = "{id:123, name:\"John\", zip:\"33025\"}";
		  JSONAssert.assertEquals(
		    "{id:123,name:\"John\"}", actual, JSONCompareMode.LENIENT);
		  ```
-
- Testing exception throws
  collapsed:: true
	- sample code
		- ```java
		  @Test
		  void testExpectedException() {
		  
		  	NumberFormatException thrown = Assertions.assertThrows(NumberFormatException.class, () -> {
		  		Integer.parseInt("One");
		  	}, "NumberFormatException was expected");
		  	
		  	Assertions.assertEquals("For input string: \"One\"", thrown.getMessage());
		  }
		  
		  @Test
		  void testExpectedExceptionWithParentType() {
		  
		  	Assertions.assertThrows(IllegalArgumentException.class, () -> {
		  		Integer.parseInt("One");
		  	});
		  }
		  
		  @Test
		  void testExpectedExceptionFail() {
		   
		  	NumberFormatException thrown = Assertions
		  				.assertThrows(NumberFormatException.class, () -> {
		  					Integer.parseInt("1");
		  				}, "NumberFormatException error was expected");
		  	
		  	Assertions.assertEquals("Some expected message", thrown.getMessage());
		  }
		  ```
		  #sample #java #junit #testing