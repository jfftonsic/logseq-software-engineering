- define configuration sources that have higher precedence than any other source used in the project
- sample:
	- ```java
	  @TestPropertySource(locations = "/other-location.properties",
	    properties = "baeldung.testpropertysource.one=other-property-value")
	  public class DefaultTest {
	    ...
	  }
	  ```
	  #sample #spring #junit