sources:: [LeonardoZ - java-concurrency-patterns](https://github.com/LeonardoZ/java-concurrency-patterns), [Shipilev - Safe Publication and Safe Initialization in Java](https://shipilev.net/blog/2014/safe-public-construction/)

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