sources:: [The Unnamed Module](https://www.logicbig.com/tutorials/core-java-tutorial/modules/unnamed-modules.html), [GeeksforGeeks Unnamed Module in Java](https://www.geeksforgeeks.org/unnamed-module-in-java/)

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