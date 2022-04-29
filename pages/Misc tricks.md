- Creating arrays for generic types
  background-color:: #533e7d
  collapsed:: true
	- for example
	- ```java
	  public class A<TYPE> {
	    TYPE[] typeArray;
	    
	    {
	      // the following line is not allowed
	      typeArray = new TYPE[10];
	      
	      // but the next is
	      typeArray = (TYPE[]) new Object[10];
	    }
	    
	  }
	  ```
- How to use for each loop in a collection that you implemented?
  background-color:: #533e7d
  collapsed:: true
	- Make your collection `Iterable` and implement your iterator.