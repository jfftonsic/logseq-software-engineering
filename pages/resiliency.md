alias:: resilient, fault-tolerant
sources:: ((627ab639-9cec-4c04-87d9-012a65393028))

- Short description
	- The capability of a system to <span class="hl-neutral-01">anticipate and cope</span> with faults.
- Sometimes it can make sense to increase the rate of faults by triggering them deliberately
	- for example, by randomly killing individual processes without warning.
- Examples of systematic error within a system
	- A <span class="hl-neutral-01">software bug</span> that causes every instance of an application server to crash when given a particular bad input.
	- A <span class="hl-neutral-01">runaway process</span> that uses up some shared resource—CPU time, memory, disk space, or network bandwidth.
	- A service that the system depends on that <span class="hl-neutral-01">slows down, becomes unresponsive, or starts returning corrupted responses</span>.
	- . <span class="hl-neutral-01">Cascading failures</span>, where a small fault in one component triggers a fault in another component, which in turn triggers further faults [10].
-