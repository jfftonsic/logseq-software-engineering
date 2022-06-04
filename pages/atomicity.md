sources:: [flylib](https://flylib.com/books/en/2.558.1/atomicity.html)
alias:: atomic

- Short description
	- Updates either succeed or fail. No partial results.
	  id:: 626eb1af-a8f7-4109-8f10-fd6a528edecb
- Compound Actions
	- Operations A and B are atomic with respect to each other if, from the perspective of a thread executing A, when another thread executes B, either all of B has executed or none of it has. An atomic operation is one that is atomic with respect to all operations, including itself, that operate on the same state.