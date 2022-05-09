sources:: https://docs.oracle.com/javase/tutorial/essential/concurrency/guardmeth.html

- Such a block begins by polling a condition that must be true before the block can proceed.
- With `Object.wait`
	- ```java
	  boolean joy; // a shared variable
	  
	  public synchronized void guardedJoy() {
	      // This guard only loops once for each special event, which may not
	      // be the event we're waiting for.
	    
	    	// ========================================================================
	    	// Note: Always invoke wait inside a loop that tests for the condition 
	    	// being waited for. Don't assume that the interrupt was for the particular 
	    	// condition you were waiting for, or that the condition is still true.
	    	// ========================================================================
	      while(!joy) {
	          try {
	              wait();
	          } catch (InterruptedException e) {}
	      }
	      System.out.println("Joy and efficiency have been achieved!");
	  }
	  
	  public synchronized notifyJoy() {
	      joy = true;
	      notifyAll();
	  }
	  ```
- The `wait/notify` mechanism is also available in `Lock` objects, through `Condition` objects