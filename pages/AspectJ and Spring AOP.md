title:: AspectJ and Spring AOP
sources:: https://guypaddock.github.io/posts/aspectj-native-syntax-with-spring-ltw-and-gradle/, [Aspect Oriented Programming with Spring](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop), [AspectJ LTW](https://www.eclipse.org/aspectj/doc/released/devguide/ltw.html), [Aspect-Oriented Programming in Spring Boot Part 2: Spring JDK Proxies vs CGLIB vs AspectJ](https://panlw.github.io/15277821532847.html)
tags:: AOP

- There are several ways with which the aspects infrastructure is implemented
	- by proxy (the Spring AOP's way)
	- Compile-time weaving
	- Post-compile weaving
	- Load-time weaving (LTW)
	-
- Spring AOP ([documentation here](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop))
	- is  proxy-based
	- no need for a special compilation process
	- does not need to control the class loader hierarchy and is thus suitable for use in a servlet container or application server.
	- less functionality than full AspectJ implementation.
	- example
		- ```java
		  @Aspect
		  @Component
		  public class MyAspect {
		  
		      @Around("execution(* foo.Clazz.method (..))")
		      public Object thisIsAnAspectMethod(ProceedingJoinPoint pjp) throws Throwable { ... }
		  }
		  
		  @Configuration
		  @EnableAspectJAutoProxy
		  // Your @ComponentScan or whichever method you are using to make the bean MyAspect findable
		  class Config {
		  }
		  ```
-
- Spring AOP + Load-time weaving ([great article](https://guypaddock.github.io/posts/aspectj-native-syntax-with-spring-ltw-and-gradle/))
	- Is a mix of proxy and bytecode-weaved aspects
	- you need:
		- a `META-INF/aop.xml` file
		  collapsed:: true
			- ```XML
			  <!DOCTYPE aspectj PUBLIC "-//AspectJ//DTD//EN" "https://www.eclipse.org/aspectj/dtd/aspectj.dtd">
			  <aspectj>
			  
			      <weaver>
			          <!-- only weave classes in our application-specific packages -->
			          <include within="org.springframework.orm.jpa.JpaTransactionManager" />
			          
			          <!-- YOU MUST INCLUDE HERE THE PLACES WHERE YOUR ASPECT CLASSES ARE IN -->
			          <include within="com.example..*"/>
			          
			      </weaver>
			  
			      <aspects>
			          <!-- weave in just this aspect -->
			          <aspect name="com.example.selfcall.TransactionAspect"/>
			      </aspects>
			  
			  </aspectj>
			  ```
		- to run your app (or test) with java agents
		  collapsed:: true
			- in your `build.gradle.kts`
				- ```kotlin
				  configurations {
				      create("runtimeAgent")
				  }
				  
				  dependencies {
				    // your dependencies
				    
				    "runtimeAgent"("org.springframework:spring-instrument")
				    "runtimeAgent"("org.aspectj:aspectjweaver")
				  }
				  
				  tasks.withType<Test> {
				      doFirst {
				          // Ensure that all of the agents we need to load at run-time happen for tests
				          configurations.getByName("runtimeAgent").onEach {
				              val jvmMutableList = jvmArgs?.toMutableList()
				              jvmMutableList?.add("-javaagent:${it.absolutePath}")
				              jvmArgs = jvmMutableList?.toList()
				          }
				      }
				  }
				  
				  tasks.withType<BootRun> {
				      doFirst {
				          // Ensure that all of the agents we need to load at run-time happen for boot run
				          configurations.getByName("runtimeAgent").onEach {
				              val jvmMutableList = jvmArgs?.toMutableList()
				              jvmMutableList?.add("-javaagent:${it.absolutePath}")
				              jvmArgs = jvmMutableList?.toList()
				          }
				      }
				  }
				  ```
		- remove the `@Component` annotation from each of the aspects (probably the author means removing from the aspects that you don't want spring to control)
		  #+BEGIN_NOTE
		  Unsure about this. I ran it now leaving `@Component` and also with a`@Bean`  at a configuration class and it worked despite that.
		  So maybe the weaving happens and spring then ignores the weaved class?
		  #+END_NOTE
-
- In the `META-INF/aop.xml` file, if you want to disable all the aspects in your code from being weaved by AspectJ (without removing the agent from the command line)
	- example file
		- ```xml
		  <!DOCTYPE aspectj PUBLIC "-//AspectJ//DTD//EN" "https://www.eclipse.org/aspectj/dtd/aspectj.dtd">
		  <aspectj>
		      <weaver>
		          <exclude within="*" />
		      </weaver>
		  
		      <aspects>
		      </aspects>
		  </aspectj>
		  ```
-
- Important Details !
	- By default, there is a single instance of each aspect within the application context
		- now, I've seen that, when using LTW via java agent, the aspect's instance is not accessible via dependency injection. And if you try to @Bean it in a @Configuration, the instance will be different.
		  I needed to get the instance to the aspect in a test, so in that case, I created a pointCut for the constructor of the test class and set the variable in the test class pointing to `this`.
		  ```java
		  @Before("execution(com.example.selfcall.TransactionalSelfCallTest.new(..)) && target(test)")
		      public void injectAspectOnTestInstance(TransactionalSelfCallTest test) {
		          test.transactionAspect = this;
		      }
		  ```
		  There are multiple (better) ways to do the same thing...
-
- Advices
	- Receiving target class and method call arguments on the advice
		- ```java
		  @Before(value="com.xyz.lib.Pointcuts.anyPublicMethod() && target(bean) && @annotation(auditable) && args(account,..)",
		          argNames="bean,auditable,account") // I don't know in which case argNames needs to be specified. Because in one of my tests I didn't set that and it worked. Maybe because it was LTW?
		  public void audit(Object bean, Auditable auditable, Account account) {
		      AuditCode code = auditable.value();
		      // ... use code and bean
		  }
		  ```