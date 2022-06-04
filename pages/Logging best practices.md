tags:: logging

- it is recommended to create your `logback-spring.xml` (or `log4j2-spring.xml`) and place it inside your resources folder. [*](https://coralogix.com/blog/spring-boot-logging-best-practices-guide/)
- consider adding: [*](https://coralogix.com/blog/spring-boot-logging-best-practices-guide/)
	- Application name.
	  A request ID.
	  The endpoint being requested (E.g /health).
- There are a few items in the default log that I would remove unless you have a specific use case for them: [*](https://coralogix.com/blog/spring-boot-logging-best-practices-guide/)
	- The ‘—’ separator.
	  The thread name.
	  The process ID.
- Logging as a Cross-Cutting Concern to Keep Your Code Clean (Using Filters and Aspects)[*](https://coralogix.com/blog/spring-boot-logging-best-practices-guide/)
	- If you are looking to create log statements related to <span style="color: orange">specific requests</span>, you should opt for using <span style="color: orange">filters</span>,
		- as they are part of the handling chain that your application already goes through for each request.
		- They are easier to write, easier to test and usually more performant than using aspects.
	- If you are considering more [[cross-cutting concerns]] (e.g. audit logging, or logging every method that causes an exception to be thrown), use [[AOP]].
	- Using a Filter to Log Every Request
		- To make use of the existing Filter, you need to supply a `CommonsRequestLoggingFilter` bean and set your logging level to debug.
- Applying Context to Your Logs Using [[MDC]][*](https://coralogix.com/blog/spring-boot-logging-best-practices-guide/)
	- For grouping logs for a specific request.
	- (This would run for every method annotated with a custom annotation, `@Audit`).
	- You can add keys/values to the map at runtime, and then reference the keys from that map in your logging pattern.
	- #+BEGIN_CAUTION
	  threads may be reused, and so you’ll need to make sure to clear your MDC after each request to avoid your context leaking from one request to the next.
	  #+END_CAUTION
		- by calling `MDC.clear()`, preferably in a finally block so that it always runs. '
	- The steps are:
		- Add a header to each request going to your API, for example, ‘tracking-id’.
			- You can generate this on the fly (I suggest using a UUID) if your client cannot provide one.
		- Create a filter that runs once per request and stores that value in the MDC.
		- Update your logging pattern to reference the key in the MDC to retrieve the value.
			- add `%X{tracking}` to your logging pattern (Replacing the word “tracking” with the key you have put in MDC)
-