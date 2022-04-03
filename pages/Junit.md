sources:: https://junit.org/junit5/docs/current/user-guide/

- **JUnit creates a new instance of each test class before executing each test method**
	- it is possible to change that, see [this](https://junit.org/junit5/docs/current/user-guide/#writing-tests-test-instance-lifecycle)
- Parameterized Tests
  collapsed:: true
	- ```java
	  @ParameterizedTest
	  @ValueSource(strings = { "racecar", "radar", "able was I ere I saw elba" })
	  void palindromes(String candidate) {
	      assertTrue(StringUtils.isPalindrome(candidate));
	  }
	  
	  @ParameterizedTest
	  @NullSource
	  @EmptySource
	  @ValueSource(strings = { " ", "   ", "\t", "\n" })
	  void nullEmptyAndBlankStrings(String text) {
	      assertTrue(text == null || text.trim().isEmpty());
	  }
	  
	  @ParameterizedTest
	  @EnumSource(ChronoUnit.class)
	  //@EnumSource(names = { "DAYS", "HOURS" })
	  //@EnumSource(mode = EXCLUDE, names = { "ERAS", "FOREVER" })
	  //@EnumSource(mode = MATCH_ALL, names = "^.*DAYS$")
	  void testWithEnumSource(TemporalUnit unit) {
	      assertNotNull(unit);
	  }
	  
	  @ParameterizedTest
	  @MethodSource("stringProvider")
	  void testWithExplicitLocalMethodSource(String argument) {
	      assertNotNull(argument);
	  }
	  
	  static Stream<String> stringProvider() {
	      return Stream.of("apple", "banana");
	  }
	  
	  @ParameterizedTest
	  @MethodSource("stringIntAndListProvider")
	  void testWithMultiArgMethodSource(String str, int num, List<String> list) {
	      assertEquals(5, str.length());
	      assertTrue(num >=1 && num <=2);
	      assertEquals(2, list.size());
	  }
	  
	  static Stream<Arguments> stringIntAndListProvider() {
	      return Stream.of(
	          arguments("apple", 1, Arrays.asList("a", "b")),
	          arguments("lemon", 2, Arrays.asList("x", "y"))
	      );
	  }
	  
	  @ParameterizedTest
	  @MethodSource("example.StringsProviders#tinyStrings")
	  void testWithExternalMethodSource(String tinyString) {
	    // test with tiny string
	  }
	  
	  @ParameterizedTest
	  @NullSource
	  @EmptySource
	  @ValueSource(strings = { " ", "   ", "\t", "\n" })
	  void nullEmptyAndBlankStrings(String text) {
	      assertTrue(text == null || text.trim().isEmpty());
	  }
	  
	  @ParameterizedTest
	  @EnumSource(ChronoUnit.class)
	  //@EnumSource(names = { "DAYS", "HOURS" })
	  //@EnumSource(mode = EXCLUDE, names = { "ERAS", "FOREVER" })
	  //@EnumSource(mode = MATCH_ALL, names = "^.*DAYS$")
	  void testWithEnumSource(TemporalUnit unit) {
	      assertNotNull(unit);
	  }
	  
	  @ParameterizedTest
	  @MethodSource("stringProvider")
	  void testWithExplicitLocalMethodSource(String argument) {
	      assertNotNull(argument);
	  }
	  
	  static Stream<String> stringProvider() {
	      return Stream.of("apple", "banana");
	  }
	  
	  @ParameterizedTest
	  @MethodSource("stringIntAndListProvider")
	  void testWithMultiArgMethodSource(String str, int num, List<String> list) {
	      assertEquals(5, str.length());
	      assertTrue(num >=1 && num <=2);
	      assertEquals(2, list.size());
	  }
	  
	  static Stream<Arguments> stringIntAndListProvider() {
	      return Stream.of(
	          arguments("apple", 1, Arrays.asList("a", "b")),
	          arguments("lemon", 2, Arrays.asList("x", "y"))
	      );
	  }
	  
	  @ParameterizedTest
	  @MethodSource("example.StringsProviders#tinyStrings")
	  void testWithExternalMethodSource(String tinyString) {
	    // test with tiny string
	  }
	  
	  @ParameterizedTest
	  @CsvSource({
	      "apple,         1",
	      "banana,        2",
	      "'lemon, lime', 0xF1",
	      "strawberry,    700_000"
	  })
	  void testWithCsvSource(String fruit, int rank) {
	      assertNotNull(fruit);
	      assertNotEquals(0, rank);
	  }
	  
	  @ParameterizedTest(name = "[{index}] {arguments}")
	  @CsvSource(useHeadersInDisplayName = true, textBlock = """
	      FRUIT,         RANK
	      apple,         1
	      banana,        2
	      'lemon, lime', 0xF1
	      strawberry,    700_000
	      """)
	  /*@CsvSource(delimiter = '|', quoteCharacter = '"', textBlock = """
	      #-----------------------------
	      #    FRUIT     |     RANK
	      #-----------------------------
	           apple     |      1
	      #-----------------------------
	           banana    |      2
	      #-----------------------------
	        "lemon lime" |     0xF1
	      #-----------------------------
	         strawberry  |    700_000
	      #-----------------------------
	      """)*/
	  void testWithCsvSource(String fruit, int rank) {
	      // ...
	  }
	             
	  @ParameterizedTest
	  @CsvFileSource(resources = "/two-column.csv", numLinesToSkip = 1)
	  void testWithCsvFileSourceFromClasspath(String country, int reference) {
	      assertNotNull(country);
	      assertNotEquals(0, reference);
	  }
	             
	             
	  @ParameterizedTest
	  @ArgumentsSource(MyArgumentsProvider.class)
	  void testWithArgumentsSource(String argument) {
	      assertNotNull(argument);
	  }
	  public class MyArgumentsProvider implements ArgumentsProvider {
	  
	      @Override
	      public Stream<? extends Arguments> provideArguments(ExtensionContext context) {
	          return Stream.of("apple", "banana").map(Arguments::of);
	      }
	  }
	  ```
	  #sample #java #junit #Testing
- Setup samples:
  collapsed:: true
	- For Gradle and Java, check out the `[junit5-jupiter-starter-gradle](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-gradle)` project.
	  For Gradle and Kotlin, check out the `[junit5-jupiter-starter-gradle-kotlin](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-gradle-kotlin)` project.
	  
	  *   For Maven, check out the `[junit5-jupiter-starter-maven](https://github.com/junit-team/junit5-samples/tree/r5.8.2/junit5-jupiter-starter-maven)` project.
- What does `@Transactional` mean if you annotate your test suite with it?
  #jpa #transaction #database
	- That every test method in your suite is surrounded by an overarching Spring transaction. This transaction will be rolled back at the end of the test method regardless of it's outcome.
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
	- sample code
	  collapsed:: true
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
-
- Display Names
  collapsed:: true
	- ```java
	  @DisplayName("A special test case")
	  class DisplayNameDemo {
	  
	      @Test
	      @DisplayName("Custom test name containing spaces")
	      void testWithDisplayNameContainingSpaces() {
	      }
	  }
	  ```
	  #sample #java #junit #Testing
-
- Assumptions
  collapsed:: true
	- failed assumptions do not result in a test failure; rather, a failed assumption results in a test being aborted.
	- ```java
	  import static org.junit.jupiter.api.Assertions.assertEquals;
	  import static org.junit.jupiter.api.Assumptions.assumeTrue;
	  import static org.junit.jupiter.api.Assumptions.assumingThat;
	  
	  import example.util.Calculator;
	  
	  import org.junit.jupiter.api.Test;
	  
	  class AssumptionsDemo {
	  
	      private final Calculator calculator = new Calculator();
	  
	      @Test
	      void testOnlyOnCiServer() {
	          assumeTrue("CI".equals(System.getenv("ENV")));
	          // remainder of test
	      }
	  
	      @Test
	      void testOnlyOnDeveloperWorkstation() {
	          assumeTrue("DEV".equals(System.getenv("ENV")),
	              () -> "Aborting test: not on developer workstation");
	          // remainder of test
	      }
	  
	      @Test
	      void testInAllEnvironments() {
	          assumingThat("CI".equals(System.getenv("ENV")),
	              () -> {
	                  // perform these assertions only on the CI server
	                  assertEquals(2, calculator.divide(4, 2));
	              });
	  
	          // perform these assertions in all environments
	          assertEquals(42, calculator.multiply(6, 7));
	      }
	  
	  }
	  ```
	  #sample #java #junit #Testing
-
- Tagging and Filtering
  collapsed:: true
	- can later be used to filter test discovery and execution
	- ```java
	  import org.junit.jupiter.api.Tag;
	  import org.junit.jupiter.api.Test;
	  
	  @Tag("fast")
	  @Tag("model")
	  class TaggingDemo {
	  
	      @Test
	      @Tag("taxes")
	      void testingTaxCalculation() {
	      }
	  
	  }
	  ```
	  #sample #java #junit #Testing