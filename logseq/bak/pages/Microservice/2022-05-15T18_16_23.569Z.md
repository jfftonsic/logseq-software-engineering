- Reasons for its origins
  heading:: true
	- New team members must quickly become productive
	- The application must be easy to understand and modify
	- [[continuous deployment]]
	- You must run multiple instances of the application on multiple machines in order to satisfy [[scalability]] and [[Availability]] requirements
	- You want to take advantage of emerging technologies (frameworks, programming languages, etc)
- Characteristics
  heading:: true
	- set of loosely coupled, collaborating services
	  background-color:: #497d46
	- [[Functional Decomposition]]
	- Each service is/has:
		- Highly maintainable and testable
		  background-color:: #497d46
			- enables rapid and frequent development and deployment
		- Loosely coupled with other services
		  background-color:: #497d46
			- enables a team to work independently the majority of time on their service(s) without being impacted by changes to other services and without affecting other services
		- Independent development and deployed
		  background-color:: #497d46
			- enables a team to deploy their service without having to coordinate with other teams
		- Capable of being developed by a small team
		  background-color:: #497d46
			- essential for high productivity by avoiding the high communication head of large teams
		- it's own database, to be decoupled
		  background-color:: #497d46
		- Consistency maintenance may be done via [[Saga]] pattern
	- Communication
	  background-color:: #787f97
		- synchronous protocols
			- [[HTTP]] / [[REST]]
		- asynchronous protocols
			- [[AMQP]]
	-
- Advantages / Benefits
  heading:: true
	- enables the continuous delivery and deployment of large, complex applications.
	  background-color:: #497d46
		- better testability
		  background-color:: #497d46
			- services are smaller and faster to test
		- better deployability
		  background-color:: #497d46
			- services can be deployed independently
		- it enables you to organize the development effort around multiple, autonomous teams. Each (so called two pizza) team owns and is responsible for one or more services. Each team can develop, test, deploy and scale their services independently of all of the other teams.
	- Each microservice is relatively small:
	  background-color:: #497d46
		- improved maintainability 
		  background-color:: #497d46
		- easier for a developer to understand
		  background-color:: #497d46
		- the IDE is faster making developers more productive
		  background-color:: #497d46
		- the application starts faster 
		  background-color:: #497d46
	- improved fault isolation. 
	  background-color:: #497d46
		- e.g. if there is a memory leak in one service then only that service will be affected. The other services will continue to handle requests. In comparison, one misbehaving component of a monolithic architecture can bring down the entire system.
	- eliminates any long-term commitment to a technology stack. 
	  background-color:: #497d46
	-
- Disadvantages / Drawbacks
  heading:: true
	- additional complexity of creating a distributed system  
	  background-color:: #793e3e
		- inter-service communication mechanism and deal with partial failure
		  background-color:: #793e3e
		- implementing requests that span multiple services is more difficult
		  background-color:: #793e3e
		- requires careful coordination between the teams  
		  background-color:: #793e3e
		- testing the interactions between services is more difficult
		  background-color:: #793e3e
		- Developer tools/IDEs are oriented on building monolithic applications and don’t provide explicit support for developing distributed applications.
		  background-color:: #793e3e
	- deployment complexity  
	  background-color:: #793e3e
		- operational complexity of deploying and managing a system comprised of many different services
	- increased memory consumption
	  background-color:: #793e3e
- When to use
  heading:: true
	- for the first versions of an application, using an elaborate, distributed architecture will slow down development. Using [[Y-Axis scaling]]  splits ([[Functional Decomposition]]) might make it much more difficult to iterate rapidly.
	- Later on, however, when the challenge is how to scale and you need to use functional decomposition, the tangled dependencies might make it difficult to decompose your monolithic application into a set of services.
- Issues to address when deciding for taking a microservices approach
  heading:: true
	- how to maintain data consistency?
		- [[2 phase-commit]] / [[distributed transactions]] is not an option
	- how to implement queries?
		- that need to retrieve data owned by multiple services.
			- The [[API Composition]] and [[Command Query Responsibility Segregation]] (CQRS)  patterns.
- Example
	- ![image.png](../assets/image_1650392557320_0.png)
- Related Patterns
	- ![image.png](../assets/image_1650392564651_0.png)
	- cross-cutting concerns
		- Microservice Chassis
		- Externalized Configuration
	- testing
		- Service Integration Contract Test
		- Service Component Test
	- observability
		- Application Metrics
		- Health-check API
		- Distributed Tracing
		- Audit logging
		- Application logging
		- Exception tracking
	- decomposition
		- decompose by business capability
		- decompose by domain-driven design subdomain
	- UI
		- Server-side page fragment composition
		- Client-side UI composition
	- security
		- Access Token
	- communication
		- API Gateway
		- Circuit Breaker
	- discovery
		- Server-side Discovery
		- Client-side Discovery
	- style
		- Messaging
		- Remote Procedure Invocation