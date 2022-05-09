- In Gradle, each source set containing Java sources can be turned into a module by adding a module-info.java file.
- Typically, in a project with Java Modules, the main source set of a subproject represents a module.
- path hierarchy
	- src
		- main
			- java
				- module-info.java
- Declaring module versions
	- Java Modules also have a version that is encoded as part of the module identity in the `module-info.class` file.
	- This version can be inspected when a module is running.
	- ```kotlin
	  //build.gradle.kts
	  version = "1.2"
	  
	  tasks.compileJava {
	      // use the project's version or define one directly
	      options.javaModuleVersion.set(provider { project.version as String })
	  }
	  ```
- Using libraries that are not modules
	- libraries that provide no module information at all
		- Gradle puts such libraries on the classpath instead of the module path.
		- The classpath is then treated as one module (the so called unnamed module) by Java.
- Plugin
	- java-library plugin
	  collapsed:: true
		- https://docs.gradle.org/current/userguide/java_library_plugin.html
		- Difference between `java` plugin and `java-library` plugin:
			- the latter introduces the <span class="hl-neutral-01">concept of an API exposed to consumers</span>.
		- **A library is a Java component meant to be consumed by other components.**
		  background-color:: #497d46
		- It’s a <span class="hl-neutral-01">very common use case in multi-project builds</span>, but also as soon as you have external dependencies.
		- java-library exposes 2 configurations
			- api
				- declare dependencies which are exported by the library API
				- transitively exposed to consumers of the library; will appear on the compile classpath of consumers.
			- implementation
				- declare dependencies which are internal to the component.
				- not leak into the consumers' compile classpath.
		- <span class="hl-bad-01-bg">[I'm unsure about this]</span>#WIP For unit (whitebox) tests that need to access the internals of a module don't add a module-info.java to the test source-set.
		- | Java Module Directive      | Gradle Configuration | Purpose                                 |
		  |----------------------------|----------------------|-----------------------------------------|
		  | requires                   | implementation       | Declaring implementation dependencies   |
		  | requires transitive        | api                  | Declaring API dependencies              |
		  | requires static            | compileOnly          | Declaring compile only dependencies     |
		  | requires static transitive | compileOnlyApi       | Declaring compile only API dependencies |
		- automatic module
			- only have a name as module description
			- export all their packages
			- can read all modules on the module path.
		- usage
			- ```kotlin
			  plugins {
			      `java-library`
			  }
			  ```
	- application plugin
		- It also has support for modules, see https://docs.gradle.org/current/userguide/application_plugin.html#sec:application_modular
	- another plugin
		- https://github.com/java9-modularity/gradle-modules-plugin