sources:: [javadoc](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/ThreadLocal.html)

- Short description
	- Wraps a variable so that each thread that accesses one (via its get or set method) has its own, independently initialized copy of the variable
- Typical usage
	- ThreadLocal instances are typically private static fields in classes that wish to associate state with a thread (e.g., a user ID or Transaction ID).
- Example
	- ```java
	       // Atomic integer containing the next thread ID to be assigned
	       private static final AtomicInteger nextId = new AtomicInteger(0);
	  
	       // Thread local variable containing each thread's ID
	       private static final ThreadLocal<Integer> threadId =
	           new ThreadLocal<Integer>() {
	               @Override protected Integer initialValue() {
	                   return nextId.getAndIncrement();
	           }
	       };
	  
	       // Returns the current thread's unique ID, assigning it if necessary
	       public static int get() {
	           return threadId.get();
	       }
	  ```