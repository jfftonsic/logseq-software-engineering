- Running a test task specifying class/method
  collapsed:: true
	- `./gradlew :<your module>:<a task of type test> --tests "<package>.<class>.<method>"`
	  `./gradlew :savings-a:test --tests "com.example.business.BalanceServiceWithDatabaseContextTest.a"`
	  #gradle #command #testing
- to be able to **re-execute tests** even when nothing changed **and** to  **show the logs of the executing app** that you are testing
  collapsed:: true
	- ```kotlin
	  tasks.withType<Test> {
	      outputs.upToDateWhen {false}
	      testLogging.showStandardStreams = true
	  }
	  ```
	  #gradle #kotlin #sample #testing
- Useful plugins:
  collapsed:: true
	- https://github.com/unbroken-dome/gradle-testsets-plugin : to create extra test source sets and tasks. E.g. for integration tests.
		- make sure the testing configuration is set like:
		  ```kotlin
		  tasks.test {
		      testLogging.showExceptions = true
		      useJUnitPlatform()
		  }
		  ```
		  because by not using the name of the task, that configuration will apply to the tasks created by the plugin also.
		  
		  then in Gradle [[Kotlin]] DSL:
		  ```kotlin
		  testSets {
		      "integrationTest"()
		  }
		  ```
		  
		  with this you have a new source set
		  ![image.png](../assets/image_1647547912702_0.png)
- [Gradle Tutorial : Part 3 : Multiple Java Projects](https://rominirani.com/gradle-tutorial-part-3-multiple-java-projects-5b1c4d1fbd8d)