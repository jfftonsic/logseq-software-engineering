- brief description
	- When there is a causal relationship between data or actions.
	  id:: 625a0470-24e5-430f-b22a-45f2bab60289
- In Happens-Before, if one action _**happens-before**_ another action, then everything done in the first action is visible to the second.
- In other terms, the write operations done by one thread are guaranteed to be visible to a read coming from another thread if the write operation _**happens-before**_ the read operation.