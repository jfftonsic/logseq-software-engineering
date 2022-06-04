alias:: reliable
sources:: ((627ab639-9cec-4c04-87d9-012a65393028))

- In the context of an entire system
	- Short description
		- The system should <span class="hl-neutral-01">continue to work correctly</span> (performing the correct function at the desired level of performance) <span class="hl-neutral-01">even in the face of adversity </span>(hardware or software faults, and even human error).
	- Expectations
		- The application performs the function that the user expected.
		- It can tolerate the user making mistakes or using the software in unexpected ways.
		- Its performance is good enough for the required use case, under the expected load and data volume.
		- The system prevents any unauthorized access and abuse.
	- [[System Design]]
	  id:: 627abad0-12ad-4066-9eea-c9f842947867
		- Approaches to make systems reliable
			- for software errors
			  collapsed:: true
				- carefully thinking about assumptions and interactions in the system
				  background-color:: #264c9b
				- testing
				  background-color:: #264c9b
				- process isolation
				  background-color:: #264c9b
				- allowing processes to crash and restart
				  background-color:: #264c9b
				- measuring, monitoring, and analyzing system behavior in production.
				  background-color:: #264c9b
			- in spite of unreliable humans
			  collapsed:: true
				- Design systems in a way that <span class="hl-neutral-01">minimizes opportunities for error</span>.
					- For example, well-designed abstractions, APIs, and admin interfaces make it easy to do “the right thing” and discourage “the wrong thing.”
						- #+BEGIN_CAUTION
						  However, if the interfaces are too restrictive people will work around them, negating their benefit, so this is a tricky balance to get right.
						  #+END_CAUTION
				- . <span class="hl-neutral-01">Decouple the environments</span> where people make the most mistakes from the environments where they can cause failures.
				- . <span class="hl-neutral-01">Test</span> thoroughly at all levels, from unit tests to whole-system integration tests and manual tests
				- Allow <span class="hl-neutral-01">quick and easy recovery</span> from human errors, to minimize the impact in the case of a failure.
					- For example,
						- make it fast to roll back configuration changes
						  background-color:: #264c9b
						- roll out new code gradually
						  background-color:: #264c9b
							- so that any unexpected bugs affect only a small subset of users
						- provide tools to recompute data .
						  background-color:: #264c9b
							- in case it turns out that the old computation was incorrect
				- Set up detailed and clear <span class="hl-neutral-01">monitoring</span>
					- such as performance metrics and error rates.
				- Implement good <span class="hl-neutral-01">management practices and training</span>
			- design for failure[*](https://tech.deliveryhero.com/our-reliability-manifesto/)
				- use protection mechanisms
					- [[timeout]]
						- Beyond a certain wait interval, a successful result is unlikely or simply too late.
					- Retry
						- Many faults are transient and may self-correct after a short delay
						  #+BEGIN_CAUTION
						  Use exponential backoff and jitter to avoid cascading failures.
						  #+END_CAUTION
					- [[Circuit Breaker]]
						- When a system is seriously struggling, failing fast is better than making clients wait to avoid running out of critical resources.
					- Fallback
						- Things will still fail, so plan what to do when that happens.
					- Throttling
						- Prevent misbehaving clients bringing your service down.
					- Ongoing load-tests, bringing your service down.
					- [[Idempotence]]
						- Multiple identical requests made to a service apply only once.
					- Recoverable
						- Your service has a process to replay messages to recover from an outage of a downstream service.
					- Dead-letter queues
						- Erroneous messages do not stop a service processing valid messages and are published to a dead-letter queue for later analysis.
					- Testing for dependency failures between systems
						- Synchronous failures include no response, slow response, error response and unexpected response (e.g. bad JSON, missing mandatory fields, wrong types).
						- Asynchronous communication, in addition, can be duplicated or out of order.
- From the point of view of data
	- Short description
		- Once an update has been applied, it will persist from that time forward until a client overwrites the update.
		  id:: 626eb1cc-f7f7-4cb6-9fa0-7bb4823086ff
-