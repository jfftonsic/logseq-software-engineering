tags:: Frameworks

-
- [[Spring Boot]]
- [[Spring Transaction]]
- [[Spring Data]]
- [[Spring Web]]
-
- Bean self reference
  collapsed:: true
	- Sometimes you need to run a method from the bean instance itself but also taking into account the java assist or wrapping that spring did on your bean.
	  
	  For example, say your class has a `@Transactional(required)` method called `m1` which needs to call a method `m2` that has a `@Transactional(requires_new)` on the same class.
	  If you want that to work as expected, you cannot just do a `this.m2(...)` on `m1`, you need to do as the example below.
		- ```java
		  @Service
		  @RequiredArgsConstructor(onConstructor = @__(@Autowired))
		  @FieldDefaults(level = AccessLevel.PRIVATE)
		  @Slf4j
		  public class JustAService {
		    	final ApplicationContext context;
		    	JustAService self;
		    
		      public JustAService getSelf() {
		          if (self == null)
		              self = context.getBean(JustAService.class);
		          return self;
		      }
		    
		      @Transactional
		      public void thisMethodDoesNotEvenNeedToBeAMethodThatRequiresClassWrapping() {
		  
		              getSelf().aMethodThatRequiresClassWrappingOfSomeSort();
		  //          aMethodThatRequiresClassWrappingOfSomeSort(idemCode, idemActor, requestTimestamp, amount); // this doesn't create the new transaction for the call
		          
		      }
		    
		      @Transactional(propagation = Propagation.REQUIRES_NEW)
		      public void aMethodThatRequiresClassWrappingOfSomeSort() {
		        //...
		      }
		        
		  }
		  
		  ```