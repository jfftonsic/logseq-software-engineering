tags:: algorithm

- How to shuffle an array to an uniformly random permutation?
	- Knuth shuffle
		- linear time
		- iterate on `i`, generate a random int `r` between 0 and `i`,  swap elements in `i` and `r`.
	- Shuffle sort (but it requires you to use a sorting algorithm)
		- generate a random real number for each element of an array and sort the array using that key.