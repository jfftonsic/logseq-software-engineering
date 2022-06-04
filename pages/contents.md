- [[Linux]]
	- [[Desktop Environment]]
		- [[XFCE]]
- <mark>Complete the following pages: </mark>
  query-table:: false
  query-properties:: [:sources :block]
  collapsed:: true
	- {{query #WIP}}
	  query-table:: false
	-
- [[Logseq plugins used with this knowledge base]]
- [[Software Engineering]]
	- [[Tips and Snippets]]
	- [[Best practices]]
	- [[Algorithm]]
		- Searching and Sorting Algorithm
			- [[union-find]]
			- [[Selection Sort]]
			- [[Insertion Sort]]
			- [[Shell Sort]]
		- [[Shuffling]]
		- [[Convex Hull]]
		- Pattern Searching Algorithm
		- String Algorithms
		- Greedy Algorithm
		- Backtracking Algorithm
		- Divide and Conquer Algorithm
		- Geometric Algorithm
		- Mathematical Algorithm
		- Bit Algorithm
		- Graph Algorithm
			- [[Minimum Spanning Tree]]
		- Randomized Algorithm
		- Branch and Bound Algorithm
		- Dynamic Programming
	- [[Data Structure]]
		- Arrays/Lists
			- Array
			- Linked List
				- LinkedList #java #[[Java Collections]]
			- Circular Linked List
			- Doubly Linked List
		- Stack
		- Queues/Deques
			- Queue
			- Priority Queue
				- [[PriorityQueue]]
			- Deque
			- ArrayDeque
			- LRU Cache
		- Trees
			- Binary Tree
			- Complete Binary Tree
			- Perfect Binary Tree
			- Balanced Binary Tree
			- Binary Search Tree
			- Balanced Binary Search Tree
			- Leftist Tree
		- Heaps
			- Binary Heap
			- Binomial Heap
			- Fibonacci Heap
			- Leftist Heap
			- K-ary Heap
		- Sets
			- Set
			- HashSet
			- LinkedHashSet
			- SortedSet
			- [[NavigableSet]]
			- TreeSet
			- [[Disjoint-set]]
			- Guava Multiset
			- Guava HashMultiset
			- Guava TreeMultiset
			- Guava LinkedHashMultiset
			- Guava ConcurrentHashMultiset
			- Guava ImmutableMultiset
			- Guava SortedMultiset
			- Guava RangeSet
		- Maps
			- HashMap
			- Hashtable
			- LinkedHashMap
			- SortedMap
			- TreeMap
			- Guava Multimap
			- Guava ListMultimap
			- Guava SetMultimap
			- Guava BiMap
			- Guava Table
		- Hash
			- Hashing
			- Trivial Hashing
			- Hash Separate Chaining
			- Hash Open Addressing
			- Double Hashing
		- Graphs
			- Graph
			- Undirected Graph
			- Directed Graph
			- Directed Acyclic Graph
			- Graph Adjacency Matrix
			- Graph Adjacency List
			- Graph Incidence Matrix
			- Graph Incidence List
		- [[Conflict-Free Replicated Data Types]]
	- [[System Design]]
		- Architectural approach
		  collapsed:: true
			- [[Microservice]]
			- [[Serverless]] #WIP
		- Architectural Patterns
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
				- Deployment Stamps #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/deployment-stamp)
			- decomposition
				- decompose by business capability
				- decompose by domain-driven design subdomain
			- UI
				- Server-side page fragment composition
				- Client-side UI composition
			- security
				- Access Token
				- Federated Identity #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/federated-identity)
				- Gatekeeper #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/gatekeeper)
			- design and implementation
				- [[Backends for Frontends]]
				- Compute Resource Consolidation #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/compute-resource-consolidation)
				- External Configuration Store #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/external-configuration-store)
				- Edge Workload Configuration #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/edge-workload-configuration)
				- Gateway Aggregation #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/gateway-aggregation)
				- Gateway Offloading #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/gateway-offloading)
				- Gateway Routing #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/gateway-routing)
				- Leader Election #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/leader-election)
				- Pipes and Filters #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/pipes-and-filters)
				- Sidecar #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/sidecar)
				- Static Content Hosting #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/static-content-hosting)
				- Strangler Fig #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/strangler-fig)
			- data #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/category/data-management)
				- [[Cache-Aside]]
				- CQRS
				- Event Sourcing
				- Index Table
				- Materialized View
				- Sharding
				- Static Content Hosting
				- Valet Key
			- communication
				- API Gateway
				- Circuit Breaker
				- Ambassador #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/ambassador)
			- discovery
				- Server-side Discovery
				- Client-side Discovery
			- style
				- Messaging
					- Asynchronous Request-Reply #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/async-request-reply)
					- Claim Check #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/claim-check)
					- Choreography #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/choreography)
					- Competing Consumers #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/competing-consumers)
					- Pipes and Filters #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/pipes-and-filters)
					- Priority Queue #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/priority-queue)
					- Publisher-Subscriber #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/publisher-subscriber)
					- Queue-Based Load Leveling #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/queue-based-load-leveling)
					- Scheduler Agent Supervisor #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/scheduler-agent-supervisor)
					- Sequential Convoy #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/sequential-convoy)
				- Remote Procedure Invocation
			- consistency
				- [[Saga]] distributed transactions pattern
				- Compensating Transaction #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/compensating-transaction)
			- resiliency
				- Anti-corruption Layer #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/anti-corruption-layer)
				- Throttling #WIP [s](https://docs.microsoft.com/en-us/azure/architecture/patterns/throttling) [s2](https://docs.microsoft.com/en-us/azure/architecture/patterns/rate-limiting-pattern)
		- Architectural Elements
			- [[Content Delivery Network]]
			- [[Domain Name System]]
			- [[Load Balancer]]
		- [[data-intensive application]]
	- [[Programming Language]]
	  collapsed:: true
		- [[Java]]
			- [[Java Concurrency]]
				- [[Java Concurrency Stress]]
			- [[Java misc tricks and tips]]
			- [[Java Streams]]
			- [[Java Collections]]
			  collapsed:: true
				- [[Java Collections Libraries]]
			- [[Java Persistence API]]
			- [[Java Transaction API]]
			- [[Java Module System]]
			- [[JUnit]]
			- [[AspectJ and Spring AOP]]
			- Java versions
				- [[Java 9]]
				- [[Java 10]]
				- [[Java 11]]
				- [[Java 12]]
				- [[Java 13]]
				- [[Java 14]]
			- Tools
				- Performance and stress related
					- [[JMH]]
					- [[Java Concurrency Stress]]
				- Launcher
					- [[Layrry]]
	- [[Framework]]
	  collapsed:: true
		- [[Spring]]
			- [[Spring Boot]]
			- [[Spring Transaction]]
			- [[Spring Data]]
			- [[Spring Web]]
			- [[Spring Integration]]
			- [[AspectJ and Spring AOP]]
		- [[Hibernate]]
	- [[Infrastructure]]
	  collapsed:: true
		- [[Docker]]
		- [[AWS]]
		- [[Ansible]]
		- [[Puppet]]
	- [[Security]]
	  collapsed:: true
		- [[Authentication]]
			- [[Authentication Scheme]]
			- [[Hypertext Transfer Protocol (HTTP/1.1): Authentication]]
			- tools
				- [[Keycloak]]
		- in frameworks
			- [[Spring Security]]
	- [[Distributed Computing]]
	- [[Concurrency]]
	  collapsed:: true
		- [[Locking]]
		- [[Lock]]
		- [[Consistency]]
		- Concurrency in databases
		  id:: 624227df-e353-4571-9700-1c9a6ed42b36
			- [[DB Transactional Locks]]
			- [[Transaction]]
			- [[Entity Manager]]
			- [[Persistence Context]]
		- in Java
			- specific collection data structures
			  collapsed:: true
				- interfaces
					- BlockingDeque
					  BlockingQueue
					  ConcurrentMap
					  ConcurrentNavigableMap
					  TransferQueue
				- concrete
					- ArrayBlockingQueue<E>
					  ConcurrentHashMap<K,V>
					  ConcurrentLinkedDeque<E>
					  ConcurrentLinkedQueue<E>
					  ConcurrentSkipListMap<K,V>
					  ConcurrentSkipListSet<E>
					  CopyOnWriteArrayList<E>
					  CopyOnWriteArraySet<E>
					  DelayQueue<E extends Delayed>
					  LinkedBlockingDeque<E>
					  LinkedBlockingQueue<E>
					  LinkedTransferQueue<E>
					  PriorityBlockingQueue<E>
					  SynchronousQueue<E>
			- concurrency constructs
			  collapsed:: true
				- CountDownLatch
				  CyclicBarrier
				  FutureTask
				  Phaser
				  RecursiveAction
				  Semaphore
				  Condition
				  Lock
				  ReadWriteLock
				  AbstractOwnableSynchronizer
				  AbstractQueuedLongSynchronizer
				  AbstractQueuedSynchronizer
				  LockSupport
				  ReentrantLock
				  ReentrantReadWriteLock
				- AtomicXXX
	- [[API]]
		- [[OpenAPI]]
		- [[REST]]
	- [[HTTP]]
	- [[Database]]
	  collapsed:: true
		- Specific databases
		  collapsed:: true
			- Open-source databases
				- [[Derby]]
				- [[Firebird]]
				- [[H2]]
				- [[HSQLDB]]
				- [[Ignite]]
				- [[MariaDB]]
				- [[MySQL]]
				- [[PostgreSQL]]
				- [[SQLite]]
				- [[YugabyteDB]]
		- {{embed ((624227df-e353-4571-9700-1c9a6ed42b36))}}
	- [[Problems / Troubleshoot]]
	- [[Testing]]
	- Tools
	  collapsed:: true
		- [[Build Tools]]
		  collapsed:: true
			- [[gradle]]
			- [[JitPack]] : ((62448ec2-3c79-49db-b279-cb949a1fc694))
			- [[Buildpacks]]
		- Distributed Coordination
			- [[Apache ZooKeeper]]
	- [[Windows]]
	  collapsed:: true
		- [[WSL]]
	- [[Diagram Generation]]
	- [[Chart]]
	- Characteristics / Properties of Systems
		- [[atomicity]]
		- [[correctness]]
		- [[efficiency]]
		- [[distributed recoverability]] #WIP
		- [[distributed serializability]]
		- [[fault-tolerance]]
		- [[global serializability]]
		- [[latency]]
		- [[liveness]]
		- [[maintainability]]
		- [[observability]] #WIP
		- [[recoverability]]
		- [[reliability]]
		- [[resiliency]]
		- [[response time]]
		- [[scalability]]
		- [[serializability]]
		- [[Single System Image]]
		- [[throughput]]
		- [[timeliness]]
	- State of Systems
	  collapsed:: true
		- [[Availability]]
		- [[Partitioning]]
	- General Definitions
		- [[consensus number]]
		- [[consensus problem]]
- [[Resources]]
- [[Miscellaneous definitions]]