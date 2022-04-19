alias:: ZooKeeper

- Short description
  collapsed:: true
	- open-source server which enables highly reliable distributed coordination.
- Long description
	- <span style="color: yellow; background-color: black; font-weight: bold">Centralized</span> service for maintaining configuration information, naming, providing distributed synchronization, and providing group services.
	- ZooKeeper works using <span style="color: yellow; background-color: black; font-weight: bold">distributed processes</span> to **coordinate** with each other through a <span style="color: yellow; background-color: black; font-weight: bold">shared hierarchical name space</span> that is modeled after a file system.
	- is more a <span style="color: yellow; background-color: black; font-weight: bold">state based system than an event system</span> (because of [this](((625b55f8-43ae-42f5-a8c1-d1c1310010a0))))
- Especially useful for
  collapsed:: true
	- distributed applications
- data is kept <span style="color: yellow; background-color: black; font-weight: bold">in memory</span> and is <span style="color: yellow; background-color: black; font-weight: bold">backed up to a log for reliability.</span>
  collapsed:: true
	- By using memory, it is very fast and can handle the high loads typically seen in chatty coordination protocols across huge numbers of processes
	- using a memory based system also mean you are limited to the amount of data that can fit in memory
		- <span style="color: orange; background-color: black; font-weight: bold">so it's not useful as a general data store. It's meant to store small bits of configuration information rather than large blobs.</span>
	- <span style="color: yellow; background-color: black; font-weight: bold">replication is used for scalability and reliability</span> which means it <span style="color: green; background-color: black; font-weight: bold">prefers applications that are heavily read based.</span>
	- typical of hierarchical systems you can
		- add nodes at any point of a tree
		- get a list of entries in a tree
		- get the value associated with an entry
		- get notification of when an entry changes or goes away
- weakness
	- id:: 625b55f8-43ae-42f5-a8c1-d1c1310010a0
	  #+BEGIN_IMPORTANT
	  You cannot reliably see every change that happens to a node
	  #+END_IMPORTANT
		- because watches are one time triggers and there is latency between getting the event and sending a new request to get a watch
		- Be prepared to handle the case where the znode changes multiple times between getting the event and setting the watch again. (You may not care, but at least realize it may happen.)
- concepts
  collapsed:: true
	- _watch event_
		- a one-time trigger, sent to the client that set the watch, which occurs when the data for which the watch was set changes.
-