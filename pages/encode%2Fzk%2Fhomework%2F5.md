- make sure you can follow the recursion example on the cairo playground
	- ```
	  func compute_sum(n: felt) -> (sum: felt) {
	      if (n == 0) {
	          // When 0 is reached, return 0.
	          return (sum=0);
	      }
	  
	      // Otherwise, call `compute_sum` recursively to compute 1 + 2 + ... + (n-1).
	      let (sum) = compute_sum(n=n - 1);
	      // Add the new value `n` to the sum.
	      let new_sum = sum + n;
	      return (sum=new_sum);
	  }
	  ```
	- does this example use [[tail recursion]]
		- https://www.cairo-lang.org/docs/how_cairo_works/functions.html?highlight=tail#tail-recursion
			- Tail recursion refers to the case when a function ends by calling a second function and immediately returning the output of this inner function without any modification.
			- no
		- [[cairo/functions]]
		  id:: ae699555-1dde-4b53-a001-383ebba3c361
	- starknet has account abstraction, so a user account is implemented as a contract that implements an abstract interface for validation and execution. ethereum eoa hardcode one such interface.