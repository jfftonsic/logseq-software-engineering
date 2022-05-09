sources:: [The Unnamed Module](https://www.logicbig.com/tutorials/core-java-tutorial/modules/unnamed-modules.html), [GeeksforGeeks Unnamed Module in Java](https://www.geeksforgeeks.org/unnamed-module-in-java/), [Oracle - Understanding java 9 modules](https://www.oracle.com/corporate/features/understanding-java-9-modules.html), [Baeldung - Java 9 modularity](https://www.baeldung.com/java-9-modularity)

- [[Gradle and Java Module System]]
- "package of Java Packages"
- Module
	- Is a group of closely related packages and resources along with a new module descriptor file.
	- Each module is responsible for its resources, like media or configuration files.
	- We can create <span class="hl-neutral-01">multi-module</span> projects comprised of a "main application" and several library modules.
	- <span class="hl-bad-01-bg">Only one module per JAR file allowed.</span>
	- put a file `module-info.java` at the root of the packages.
- Service
	- implementation of a specific interface or abstract class that can be consumed by other classes.
- Descriptor
	- Name – the name of our module
	- Dependencies – a list of other modules that this module depends on
	- Public Packages – a list of all packages we want accessible from outside the module
		- We <span class="hl-neutral-01">need to list all packages we want to be public</span> because <span class="hl-bad-01-bg">by default all packages are module private</span>.
	- Services Offered – we can <span class="hl-neutral-01">provide service implementations that can be consumed by other modules</span>
	- Services Consumed – allows the <span class="hl-neutral-01">current module to be a consumer of a service</span>
	- Reflection Permissions – <span class="hl-neutral-01">explicitly allows other classes to use reflection to access the private members of a package</span>
- Module Types
	- System Modules
		- Modules listed when we run the `list-modules` command. Java SE and JDK modules.
	- Application Modules
		- Modules that we build.
		- Named and defined in the compiled `module-info.class` file included in the assembled JAR.
	- Automatic Modules
		- We can include <span class="hl-neutral-01">unofficial modules by adding existing JAR files</span> to the module path. The name of the module will be derived from the name of the JAR.
		- Automatic modules will have <span class="hl-neutral-01">full read access to every other module</span> loaded by the path.
	- Unnamed Module
		- <span class="hl-neutral-01">When a class or JAR is loaded onto the classpath, but not the module path</span>, it's automatically added to the unnamed module. It's a catch-all module to maintain <span class="hl-neutral-01">backward compatibility</span> with previously-written Java code.
- module-info.java examples
	- ```java
	  module my.module { 
	      requires module.name; // now the module has both a runtime and a compile-time dependency with module.name 
	    
	  	// required compile-time dependency, optional run-time dependency; 
	  	// (code that references another module, but that users of our 
	  	// library will never want to use.) 
	  	requires static module.name2; 	
	    									
	    									
	    
	  	// force any downstream consumers also to read our required dependencies 
	  	requires transitive module.name3; 
	  
	  	// expose all public members of the named package 
	  	exports com.my.package.name; 
	    	
	  	// restrict which modules have access to our APIs using the exports…to directive 
	  	export com.my.package.name2 to com.specific.package; 
	  
	  	// services our module consumes; the class name we use is either the interface 
	  	// or abstract class of the service, not the implementation class 
	  	uses class.name; 
	  	provides MyInterface with MyInterfaceImpl; 
	  } 
	  
	  // If you want the module to be reflectable, we now have to explicitly grant permission
	  // for other modules to reflect on our classes. 
	  open module my.module { 
	  } 
	  
	  // we also can use the opens directive to expose only specific packages. 
	  module my.module { 
	    opens com.my.package; 
	    
	    // also selectively open our packages to a pre-approved list of modules 
	    opens com.my.package2 to moduleOne, moduleTwo, etc.; 
	  } 
	  ```
- Summary of the unnamed module
  The unnamed module does not have module-info.java.
  Or in other words, the classes which do not have modules are promoted to the unnamed module.
  The unnamed module 'requires' all other modules and 'exports' all its packages.
  The classes which were written in the older versions but now running in Java 9 environment will continue to work because they become the members of the unnamed module.
  If we want to run old code in Java 9 environment without migrating them to the module system, we should run them using class path (not module path)
- What's the difference between --add-exports and --add-opens in Java 9?
  sources:: https://stackoverflow.com/questions/44056405/whats-the-difference-between-add-exports-and-add-opens-in-java-9
	- With --add-exports the package is exported, meaning all public types and members therein are accessible at compile and run time.
	  With --add-opens the package is opened, meaning all types and members (not only public ones!) therein are accessible at run time.
	  So the main difference at run time is that --add-opens allows "deep reflection", meaning access of non-public members. You can typically identify this kind of access by the reflecting code making calls to setAccessible(true).
	  
	  A command to find which module provides which package: `java --list-modules | tr @ " " | awk '{ print $1 }' | xargs -n1 java -d` the name of the module will be shown with the @ while the name of the packages without it
- You can use `open module` to open all packages (internal or not) to all modules.
  sources:: https://stackoverflow.com/questions/47866761/can-a-module-info-java-opens-statement-include-a-package-and-all-sub-packages
	- ```java
	  open module foo.microservice {
	    requires spring.core;
	    requires spring.beans;
	    requires spring.context;
	    requires java.sql; // required for Spring Annotation based configuration :(
	  
	    exports foo.microservice.configuration;
	    exports foo.microservice.controllers;
	    exports foo.microservice.models;
	    exports foo.microservice.services;
	  }
	  ```
- [JLS defines a module declaration](https://docs.oracle.com/javase/specs/jls/se9/html/jls-7.html#jls-7.7), syntax:
  sources:: https://stackoverflow.com/questions/47866761/can-a-module-info-java-opens-statement-include-a-package-and-all-sub-packages
	- ```text
	  ModuleDirective:
	       requires {RequiresModifier} ModuleName ;
	       exports PackageName [to ModuleName {, ModuleName}] ;
	       opens PackageName [to ModuleName {, ModuleName}] ;
	       uses TypeName ;
	       provides TypeName with TypeName {, TypeName} ; 
	  ```
- no wildcards are allowed in the package name