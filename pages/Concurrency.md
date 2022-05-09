sources:: [LeonardoZ - java-concurrency-patterns](https://github.com/LeonardoZ/java-concurrency-patterns), [Shipilev - Safe Publication and Safe Initialization in Java](https://shipilev.net/blog/2014/safe-public-construction/)

- Thread Interference
	- Interference happens when two operations, running in different threads, but acting on the same data, interleave.
	- This means that the two operations consist of multiple steps, and the sequences of steps overlap.
- Memory consistency errors
	- occur when different threads have inconsistent views of what should be the same data.
	- The key to avoiding memory consistency errors is understanding the [[happens-before]] relationship.
	- This relationship is simply a guarantee that memory writes by one specific statement are visible to another specific statement.
	  id:: 626c74e4-bd6b-4f84-819d-820538b54920
- Characteristics
  heading:: true
	- [[Commitment Ordering]]
	- [[Strict Commitment Ordering]]
	- [[global serializability]] #WIP <span class="hl-bad-01-bg">(is it the same as [[global ordering]] ?? )</span>
	- [[global ordering]]
	- [[generalized autonomy]]
- Concurrency Control Protocols
  heading:: true
	- [[Lock-Based Protocol]]
	- [[Two Phase Locking Protocol]]
		- ((626ec4b3-c328-48b5-9dcc-9c205c80678c))
		- ((626ec541-a10f-4618-b747-54ad77e77301))
		- ((626ec552-72ba-4b5a-9415-53dd875d5eb0))
		- ((626ec559-af39-4e59-acd1-ca7f998e01af))
	- [[Timestamp-Based Protocol]] - ((626ec6fb-bda7-4cb2-bf73-152fa72a5559))
	- [[Validation-Based Protocol]]
- Coordination Strategies
  heading:: true
  collapsed:: true
	- [[Guarded Blocks]]
	  heading:: true
		- {{embed [[Guarded Blocks]]}}
- concurreny constructs
  heading:: true
  collapsed:: true
	- [[Lock]]
	- [[Fencing Token]] - ((625dadd2-8136-4b38-95e4-84b9e6f225e1))
- Lock approaches
  heading:: true
  collapsed:: true
	- Intrinsic Lock / Monitor Lock
	  heading:: true
		- https://docs.oracle.com/javase/tutorial/essential/concurrency/locksync.html
		- in java = `synchronized`
	- Explicit
	  heading:: true
		- Reentrant
		- ReadWrite
- Synchronizers
  heading:: true
  collapsed:: true
	- Latches
	- Semaphores
	- Barriers
- Synchronized collections
  heading:: true
- Concurrent collections
  heading:: true
  collapsed:: true
	- alternative to the Synchronized Collections, supposedly performs better
- Executors
  heading:: true
  collapsed:: true
	- Thread Pool
		- Fixed Thread Pool
		- Cached Thread Pool
		- Single Thread Pool
		- Scheduled Thread Pool
		- Single Thread Scheduled
		- Work-Stealing Pool
- Atomics
  heading:: true
- Futures
  heading:: true
- Fork/Join Framework
  heading:: true
- Database related
  heading:: true
  collapsed:: true
	- [[DB Transactional Locks]]
	- [[Transaction]]
- Concurrency Patterns / Strategies
  heading:: true
	- Limiting state to a single thread (Swing, Vaadin, JMonkey Engine, Apache Wicket use this strategy)
	- Duplicate state to make it local
	- Make state manipulation and query `synchronized`
	- Read/Write locks
	- double checked locking
	  collapsed:: true
		- Short description
			- Do an initial validation, to decide if you should enter a locked context, then lock, then validate again to do the action on a shared variable. <span style="color: red; background-color: black; font-weight: bold">Careful, the variable and initialization must be static! see <a href="https://shipilev.net/blog/2014/safe-public-construction/">this</a></span>
		- Example
			- Singleton lazy initialization
				- ```java
				      private static HospitalOperation _instanceForDoubleCheckLocking;
				  
				  	// Double Checked Locking- Synchronized Block
				      public static HospitalOperation
				      getInstanceSynchronizedBlockWay()
				      {
				   
				          // Checking for double locking
				          if (_instanceForDoubleCheckLocking == null)
				              synchronized (HospitalOperation.class)
				              {
				                  if (_instanceForDoubleCheckLocking == null)
				                      _instanceForDoubleCheckLocking
				                          = new HospitalOperation();
				              }
				   
				          return _instanceForDoubleCheckLocking;
				      }
				  ```
	- Atomic Compound Actions
	  collapsed:: true
		- Short description
			-
		- Motivations
			- Compounded actions are actions that depends on a sequence of events. They need to be executed atomically as a single unit which totally fails or complete. check-then-act, read-modify-write and compare- and-swap are common idioms in concurrent programming that can cause race conditions if not treated right.
		- Intent
			- Prevent race condition issues while using intrinsic locking mechanisms when executing compound actions; protect every path where the involved variables are used.
		- Applicability
			- read-modify-write (i++ operator), check-then-act (lazy initialization, singleton), compare-and-swap (Stacks).
		- Example
			- ```java
			  	private int value;
			  
			  	public synchronized int getValue() {
			  		return value;
			  	}
			  
			  	public synchronized int compareAndSwap(int expected, int newValue) {
			  		int old = value;
			  		if (old == expected) {
			  			value = newValue;
			  		}
			  		return old;
			  	}
			  ```
	- Lock Split
	  collapsed:: true
		- Short description
			- In a class with multiple shared, mutable, independent variables, you put one lock for each of them or to groups of them.
		- Motivations
			- If you have shared, mutable, independent and hot variables, you can increase performance by giving each variable or variable group it's own lock.
		- Intent
			- If the variables or variable groups are independent in terms of logic and usage, we guard their state by assigning a lock to each one of then. We protect all paths that interacts with each variable or variable group, creating a thread safe class that is efficiently in term of lock contention and other hazards like race conditions.
		- Applicability
			- Classes where you have shared, mutable, independent and hot variables or variables groups, where one single lock will be inefficiently.
	- Fixed Lock Ordering
	  collapsed:: true
		- Short description
			- When locking more than one Lock at the same time, define a fixed order to make the lock calls.
		- Motivations
			- Acquiring locks in a non fixed-order can deadlock if they're called at the same time, but with the inverse order.
		- Intent
			- Create a fixed-ordered locking mechanism to prevent possible deadlocks. 
			  We define a value to the locks objects and use comparisons to establish a fixed order bases on who is greater or lesser.
		- Applicability
			- Every time when acquiring more than one lock.
		- Example
			- ```java
			  public class CoinTransfer {
			  
			  	static class Player {
			  		private int id;
			  		private String name;
			  		private BigInteger coins;
			  
			  		public int getId() {
			  			return id;
			  		}
			  
			  		public void setId(int id) {
			  			this.id = id;
			  		}
			  
			  		public String getName() {
			  			return name;
			  		}
			  
			  		public void setName(String name) {
			  			this.name = name;
			  		}
			  
			  		public BigInteger getCoins() {
			  			return coins;
			  		}
			  
			  		public void depositCoins(BigInteger amount) {
			  			if (amount.intValue() < -1)
			  				throw new IllegalArgumentException("Amount can't be negative");
			  			this.coins = this.coins.add(amount);
			  		}
			  
			  		public void withdrawCoins(BigInteger amount) {
			  			if (amount.intValue() < -1)
			  				throw new IllegalArgumentException("Amount can't be negative");
			  			this.coins = this.coins.subtract(amount);
			  		}
			  	}
			  
			  	public void transferBetweenPlayers(Player playerFrom, Player playerTo, BigInteger amount) {
			  		var from = playerFrom.getId();
			  		var to = playerTo.getId();
			  		if (from < to) {
			  			synchronized (playerFrom) {
			  				synchronized (playerTo) {
			  					transferLogic(playerFrom, playerTo, amount);
			  				}
			  			}
			  		} else {
			  			synchronized (playerTo) {
			  				synchronized (playerFrom) {
			  					transferLogic(playerFrom, playerTo, amount);
			  				}
			  			}
			  		}
			  	}
			  
			  	public void transferLogic(Player playerFrom, Player playerTo, BigInteger amount) {
			  		playerFrom.withdrawCoins(amount);
			  		playerTo.depositCoins(amount);
			  	}
			  }
			  ```
		-
	- Thread Local Confinement
	  collapsed:: true
		- Short description
			- Put a variable in a [[ThreadLocal]] so that each thread that calls `get` gets its own newly initialized instance of the variable.
		- Motivations
			- To have thread-safe code when using non thread-safe classes.
		- Intent
			- Use [[ThreadLocal]] to confine instances in a per-thread model, keeping a object copy to each thread.
		- Applicability
			- Use for non thread-safe objects that needs to be shared across threads.
		- Example
			- ```java
			  public class ThreadSafeDateFormat {
			  
			  	private static final ThreadLocal<SimpleDateFormat> threadLocalDateFormat = new ThreadLocal<SimpleDateFormat>() {
			  		@Override
			  		protected SimpleDateFormat initialValue() {
			  			return new SimpleDateFormat("DD/MM/YYYY HH:mm:ss");
			  		}
			  	};
			  
			  	public String format(Date date) {
			  		return threadLocalDateFormat.get().format(date);
			  	}
			  }
			  ```
	- Immutable Object with Volatile Reference
	  collapsed:: true
		- Short description
			- Using a volatile variable pointing to instance of an immutable class to prevent visibility and atomic related hazards.
		- Motivations
			- Mutable state is troublesome in concurrent environments for being vulnerable to visibility and atomic related hazards, like inconsistent state objects, lost updates, stale values and others.
			- #+BEGIN_IMPORTANT
			  IMPORTANT: references to immutable objects are not thread-safe.
			  #+END_IMPORTANT
		- Intent
			- Make all fields final
			- don't use any method that can mutate the internal state of the object
			- don't expose the object until it's full created
			- make the class final, prohibiting extension.
			- It's important to refer to an immutable object using a volatile field, because it has visibility guarantees (every change to the immutable object variable will be made visible to all threads), and any synchronizing mechanism to ensure thread-safety for the reference.
		- Applicability
			- When sharing a object, use Immutable Objects being referenced by a volatile fields.
		- Example
			- I think there is a problem with the example below. The `payload` content is mutable.
			  background-color:: #793e3e
			- ```java
			  public class EventKeeper {
			  
			  	private volatile Event lastEvent = new Event(null, null, null, null, null);
			  
			  	public void acceptEvent(String name, String type, String username, byte[] payload) {
			  		switch (type) {
			  		case "STORAGE":
			  			lastEvent = new Event(UUID.randomUUID().toString(), name, type, username, payload);
			  			break;
			  		default:
			  			break;
			  		}
			  	}
			  
			  	public Event getLastEvent() {
			  		return lastEvent;
			  	}
			  
			  	static final class Event {
			  		private final String id;
			  		private final String name;
			  		private final String type;
			  		private final String username;
			  		private final byte[] payload;
			  
			  		public Event(String id, String name, String type, String username, byte[] payload) {
			  			super();
			  			this.id = id;
			  			this.name = name;
			  			this.type = type;
			  			this.username = username;
			  			this.payload = payload;
			  		}
			  
			  		public String getId() {
			  			return id;
			  		}
			  
			  		public String getName() {
			  			return name;
			  		}
			  
			  		public String getType() {
			  			return type;
			  		}
			  
			  		public String getUsername() {
			  			return username;
			  		}
			  
			  		public byte[] getPayload() {
			  			return payload;
			  		}
			  
			  	}
			  
			  }
			  ```
	- Safe Initialization
	  sources:: https://shipilev.net/blog/2014/safe-public-construction/
	  collapsed:: true
		- Short description
			- Safe initialization makes all values initialized in constructor visible to all readers that observed the object, regardless of whether the object was safely published or not.
		- How?
			- Java Memory Model guarantees this if all the fields in object are final and there is no leakage of the under-initialized object from constructor.
	- Safe Lazy Initialization
	  collapsed:: true
		- Short description
		- Motivations
			- Due to the Java Memory Model, some lazy initialization patterns can be unsafe.
		- Intent
			- Safe initialize our object using the holder idiom. Memory writes made during static initialization are automatically visible to all threads.
		- Applicability
			- Always when you want to initialize an object in a concurrent code execution.
		- Example
			- background-color:: #793e3e
			  ```java
			  public class SafeInitializationHolder {
			  	private static class ResourceHolder {
			        	// it may seem weird that this is lazy, but it is.
			  		public static Object resource = new Object();
			  	}
			  
			  	public static Object getResource() {
			  		return ResourceHolder.resource;
			  	}
			  }
			  ```
	- Safe Publishing
	  collapsed:: true
		- Short description
		- Motivations
			- Publishing an object is making it visible to other parts of the code, outside the current scope, showing the reference to it. If not made properly, the published object can be in an inconsistent state due to the Java Memory Model.
		- Intent
			- Publish an object safely, both the reference to the object and the object state must be made visible to other threads at the same time.
		- Applicability
			- Always when you want to make an object visible to other in a concurrent code execution.
		- Examples
			- Exchange the reference through a properly locked field
			  collapsed:: true
				- ```java
				  public class SynchronizedCLFactory {
				    private Singleton instance;
				  
				    public Singleton get() {
				      synchronized(this) {
				        if (instance == null) {
				          instance = new Singleton();
				        }
				        return instance;
				      }
				    }
				  }
				  ```
			- Use static initializer to do the initializing stores
				- Class initialization lock provides the mutual exclusion during the class initialization, that is, only a single thread can initialize the static fields.
				- ```java
				  public class HolderFactory {
				    //Note this thing is lazy, since we do not initialize Holder until the first call to get():
				    public static Singleton get() {
				      return Holder.instance;
				    }
				  
				    private static class Holder {
				      public static final Singleton instance = new Singleton();
				    }
				  }
				  ```
			- Exchange the reference via a volatile field (JLS 17.4.5), or as the consequence of this rule, via the AtomicX classes
				- ```java
				  public class SafeDCLFactory {
				    // Volatile reads and writes of instance yield synchronization actions that are totally ordered, 
				    // and consistent with program order. Therefore two consecutive reads of instance are guaranteed 
				    // to see the same value, in the absence of intervening write to instance.
				    private volatile Singleton instance;
				  
				    public Singleton get() {
				      if (instance == null) {  // check 1
				        synchronized(this) {
				          if (instance == null) { // check 2
				            instance = new Singleton();
				          }
				        }
				      }
				      return instance;
				    }
				  }
				  ```
			- Initialize the value into a final field
				- ```java
				  public class FinalWrapperFactory {
				    private FinalWrapper wrapper;
				  
				    public Singleton get() {
				      FinalWrapper w = wrapper;
				      if (w == null) { // check 1
				        synchronized(this) {
				          w = wrapper;
				          if (w == null) { // check2
				            w = new FinalWrapper(new Singleton());
				            wrapper = w;
				          }
				        }
				      }
				      return w.instance;
				    }
				  
				    private static class FinalWrapper {
				      public final Singleton instance;
				      public FinalWrapper(Singleton instance) {
				        this.instance = instance;
				      }
				    }
				  }
				  ```
			- Another example using static, from another source
				- #+BEGIN_QUOTE
				  In this example, we're using static fields, but AtomicReference, Volatile and Final Fields can also an option.
				  #+END_QUOTE
				- ```java
				  public class SafePublishing {
				  
				  	public static Object object;
				  
				  	static {
				  		// use static field or a static block to initialize
				  		// static initialization is safe because it's done automatically locked.
				  		object = new Object();
				  	}
				  
				  }
				  ```
		-
	- Resource Pool
	  collapsed:: true
		- Short description
		  collapsed:: true
			- Given a set of limited resources of a kind, establishes a mechanism for acquisition and release of one or more resources by a process.
		- Motivations
		  collapsed:: true
			- You have a limited set of resources of the same kind and want to access operations they support concurrently while respecting the availability of resources.
		- Example
			- [code source](https://github.com/LeonardoZ/java-concurrency-patterns/tree/master/src/main/java/br/com/leonardoz/patterns/resource_pool)
			  ```java
			  class ResourcePool<T> {
			  
			  	private final static TimeUnit TIME_UNIT = TimeUnit.SECONDS;
			  	private Semaphore semaphore;
			  	private BlockingQueue<T> resources;
			  
			  	public ResourcePool(int poolSize, List<T> initializedResources) {
			  		this.semaphore = new Semaphore(poolSize, true);
			  		this.resources = new LinkedBlockingQueue<>(poolSize);
			  		this.resources.addAll(initializedResources);
			  	}
			  
			  	public T get() throws InterruptedException {
			  		return get(Integer.MAX_VALUE);
			  	}
			  
			  	public T get(long secondsToTimeout) throws InterruptedException {
			  		semaphore.acquire();
			  		try {
			  			T resource = resources.poll(secondsToTimeout, TIME_UNIT);
			  			return resource;
			  		} finally {
			  			semaphore.release();
			  		}
			  	}
			  
			  	public void release(T resource) throws InterruptedException {
			  		if (resource != null) {
			  			resources.put(resource);
			  			semaphore.release();
			  		}
			  	}
			  }
			  
			  public class ResourcePoolUsage {
			  	public static void main(String[] args) {
			  		var executor = Executors.newCachedThreadPool();
			  		var pool = new ResourcePool<Integer>(15,
			  				Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 0, 10, 11, 12, 13, 14));
			  		var random = new Random();
			  		for (int i = 0; i < 30; i++) {
			  			executor.execute(() -> {
			  				try {
			  					var value = pool.get(60);
			  					System.out.println("Value taken: " + value);
			  					Thread.sleep(random.nextInt(5000));
			  					pool.release(value);
			  					System.out.println("Value released " + value);
			  				} catch (InterruptedException e) {
			  					e.printStackTrace();
			  				}
			  			});
			  		}
			  		executor.shutdown();
			  	}
			  }
			  ```
	- Condition Queues
		- sources:: https://flylib.com/books/en/2.558.1/using_condition_queues.html
		- Short description
			- Seems to me this just means using `synchronized` + `wait or notify` or `Locks` + `Condition` objects. The <span class="hl-neutral-01">condition queue object</span> is the object on which `wait` and `notify` are invoked.
		- To use it correctly, identify the condition predicates that the object may wait for.
		- <span class="hl-bad-01-bg">When using condition waits (Object.wait or Condition.await):</span>
			- Always have a condition predicate, some test of object state that must hold before proceeding;
			- Always test the condition predicate before calling wait, and again after returning from wait;
			- Always call wait in a loop;
			- Ensure that the state variables making up the condition predicate are guarded by the lock associated with the condition queue;
			- Hold the lock associated with the the condition queue when calling wait, notify, or notifyAll;
			- Do not release the lock after checking the condition predicate but before acting on it.
			- make sure that someone will perform a notification whenever the condition predicate becomes true.
		- Issues
			- Waking up too soon
				- `wait` returning does not necessarily mean that the condition predicate the thread is waiting for has become true.
			- Missed Signals
				- Occurs when a thread must wait for a specific condition that is already true, but fails to check the condition predicate before waiting. <span class="hl-neutral-02-bg">Notification is not "sticky"</span>.
			- Subclass Safety
				- A state-dependent class should either fully expose (and document) its waiting and notification protocols to subclasses, or prevent subclasses from participating in them at all.
		- Example
			- ```java
			  void stateDependentMethod() throws InterruptedException {
			   // condition predicate must be guarded by lock
			   synchronized(lock) {
			   	while (!conditionPredicate())
			   		lock.wait();
			   	// object is now in desired state
			   }
			  }
			  ```
		-
	- PatternName
		- Short description
		- Motivations
		- Intent
		- Applicability
		- Example
		-
- Related concepts
  heading:: true
  collapsed:: true
	- [[Consistency]]
	- [[concurrent data object]] - ((625b5711-4fbb-4514-b49e-ce4648d1a5f6))
		- [[wait-free implementation of a concurrent data object]]
- https://www.geeksforgeeks.org/concurrency-control-in-dbms
- https://www.cs.princeton.edu/courses/archive/spr18/cos518/docs/L3-strong-consistency.pdf
-
-