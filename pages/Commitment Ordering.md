alias:: CO, Dynamic Atomicity, commitment-ordered
sources:: https://sites.google.com/site/yoavraz2/home/theory-of-commitment-ordering, [Wikipedia - Global serializability](https://en.wikipedia.org/wiki/Global_serializability)

- Short description
	- de facto standard to achieve [[global serializability]] across ([[SS2PL]]   based) database systems
- the most general (a necessary condition) that guarantees [[global serializability]]
- Allows optimistic (non-blocking) implementations.
- low overhead, general solution for [[global serializability]] (and [[distributed serializability]] )
- the chronological order of commitment events of transactions is compatible with the precedence order of the respective transactions.
- A fundamental part of the solution is [[Atomic Commitment Protocol]]
- database systems involved
	- <span class="hl-neutral-01">do not share concurrency control information</span> beyond [[Atomic Commitment Protocol]]
	- have no knowledge of whether transactions are global or local ( <span class="hl-neutral-02-bg">the database systems are  autonomous</span> )
- CO compliant database systems (with any different concurrency control types) can <span class="hl-neutral-01">transparently join</span> such [[SS2PL]] based solutions for [[global serializability]] .
- <span class="hl-neutral-02-bg">Locking based global deadlocks are resolved automatically</span> in a CO based multi-database environment
- Benefits of CO for global serializability
  heading:: true
	- Seamless, low overhead integration with any concurrency control mechanism
		- neither changing any transaction's operation scheduling or blocking it, nor adding any new operation.
		- The overheads incurred by the CO solution are
			- locally detecting conflicts
				- which is already done by any known serializability mechanism, both pessimistic and optimistic
			- locally ordering in each database system both the (local) commits of local transactions
			- the voting for atomic commitment of global transactions.
	- Heterogeneity
		- Global serializability is achieved across multiple transactional objects (e.g., database management systems) with different (any) concurrency control mechanisms, without interfering with the mechanisms' operations.
	- Modularity
		- Transactional objects can be added and removed transparently.
	- Autonomy of transactional objects
		- No need of conflict or equivalent information distribution (e.g., local precedence relations, locks, timestamps, or tickets; no object needs other object's information).
	- Scalability
		- With "normal" global transactions, computer network size and number of transactional objects can increase unboundedly with no impact on performance, and Automatic global deadlock resolution.
	- automatically resolves global deadlocks due to locking
- Variants
	- [[Extended CO]]
	- [[Multi-version CO]]
	- [[Vote ordering]]
-
- [[Extended CO]] and [[Multi-version CO]] both use additional information for <span class="hl-neutral-01">relaxing CO constraints and achieving better concurrency and performance</span> .