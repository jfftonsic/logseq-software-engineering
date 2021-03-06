sources:: https://www.lambdatest.com/blog/junit5-mockito-tutorial/, https://javadoc.io/doc/org.mockito/mockito-core/latest/org/mockito/Mockito.html

- If you want to use annotations
  collapsed:: true
	- ```java
	  // With Junit 5
	  @ExtendWith(MockitoExtension.class)
	  class BalanceControllerTest {
	      @Mock
	      IBalanceService balanceService;
	    ...
	  }
	  
	  // Want to set the strictness?
	   @MockitoSettings(strictness = Strictness.STRICT_STUBS)
	   public class ExampleTest {
	  //...
	   }
	    
	  // Want to receive a mocked object as a parameter to a @Test method? (when using @ExtendWith on the class). It also works as contructor parameters.
	       @Test
	       public void hasLocalMockInThisTest(@Mock List<Integer> localList) { /*...*/ }
	  
	  // with Junit 4
	  @RunWith(MockitoJUnitRunner.class) 
	  public class DemoMock { 
	  ..... 
	  }
	  ```
- `@Mock` instead of `Mockito.mock(...)`
  collapsed:: true
	- ```java
	  @Mock
	  ToDoService serviceMock;
	  ```
- Inject mocks
  collapsed:: true
	- ```java
	  @Mock
	  Map<String, String> Countries;
	   
	  @InjectMocks
	  MyDictionary dic = new Continent();
	   
	  @Test
	  public void UseInjectMocksAnnotation() {
	     Mockito.when(Countries.get("India")).thenReturn("asia");
	   
	     assertEquals("asia", dic.getContinent("India"));
	  }
	  ```
- Spy: used to create a real object and spy on that real object. This would help to call all the object methods while still tracking every interaction that is being mocked.
  collapsed:: true
	- ```java
	  @Spy
	  List<String> myList = new ArrayList<String>();
	   
	  @Test
	  public void usingSpyAnnotation() {
	     myList.add("Hello, This is LambdaTest");
	     Mockito.verify(spyList).add("Hello, This is LambdaTest");
	     assertEquals(1, spyList.size());
	  }
	  ```
- Captor: used to create an `ArgumentCaptor` instance to capture method argument values for further assertions.
	- ```java
	  @Mock
	  HashMap<String, Integer> MyMap;
	  @Captor
	  ArgumentCaptor<String> keyCaptor;
	  @Captor
	  ArgumentCaptor<Integer> valueCaptor;
	  @Test
	  public void ArgumentCaptorTest()
	  {
	     hashMap.put("A", 10);
	     Mockito.verify(MyMap).put(keyCaptor.capture(), valueCaptor.capture());
	     assertEquals("A", keyCaptor.getValue());
	     assertEquals(new Integer(10), valueCaptor.getValue());
	  }
	  ```
- When you need to do a different kind of validation on an argument:
  collapsed:: true
	- ```java
	  argThat(arg -> arg.compareTo(new BigDecimal(amount)) == 0)
	  ```
- Mockito has at this moment ([[Mar 18th, 2022]]) support for static methods, but it requires the usage of another dependency. So, it seems you don't need PowerMock anymore.
  collapsed:: true
	- ```java
	   assertEquals("foo", Foo.method());
	   try (MockedStatic mocked = mockStatic(Foo.class)) {
	   mocked.when(Foo::method).thenReturn("bar");
	   assertEquals("bar", Foo.method());
	   mocked.verify(Foo::method);
	   }
	   assertEquals("foo", Foo.method());inline
	  ```