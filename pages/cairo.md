- general description
	- general computation on a blockchain allow a different relationship between results of interactions of stakeholders and  institutional oversight
	- this different relationship that seeks to lessen the demand for 'fiat' institutional oversight to establish the legitimacy of results of interactions of stakeholders leads to a scaling problem.
	- the scaling problem arises because the attempt to minimize institutional oversight leads to the verification of the results of interaction to be done across a permissionless class of participants, where each participant validates a computation.
	- [[interactive proof protocols]] allow a different approach to verification of computational claims, one that is more efficient.
	- cairo uses a method a particular cyptographic method, [[STARK]], to verify claims about the execution in one particular Turing complete model of computation, the [[Cairo machine]]
	  id:: 635a9851-a634-478f-aeb8-cce629856498
	- cairo uses STARK to represent a trace of an execution of a CAIRO machine as a table of values in a polynomial via an encoding the claims of an execution of a CAIRO program to an [[Algebraic Intermediate Representation]]
	  id:: 635a9851-7da4-41d2-83c7-40eb2bf8353e
		- the claim that a program has executed successfully is expressed as a claim about the existence of a solution to a family of [[polynomial]]s
	- ((641354b8-bcc3-42bb-8946-520c97786410))
		- in solidity
			- we might write a statement to extract an amount from a balance
		- in cairo
			- we would write a statement to check that for the parties involved the sum of balances hasn't changed
			- each of the values in memory are chosen b
- memory model
	- ((641354b8-bcc3-42bb-8946-520c97786410))
		- you run your logic and assert that is has been done correct
	- read-only nondeterministic memory
		- the value for each memory cell is chosen by the prover, but it cannot change during a Cairo program execution
	- memory cell is specified by square brackets
	- ``` 
	  [x] \\ gives us the value at address x
	  [1] == 13 \\ can mean either
	  \\ set the value at address 1 to be 13 if it hasn't been already been set
	  \\ test whether the value at address 1 is 13
	  ```
	- there are three registers used
		- ap -- the [[allocation pointer]], to show where unused memory starts
		- fp -- the [[frame pointer]], this points to the function we are in, and the variables in the function then offsets from that
		- pc -- the [[program counter]], this gives us the current instruction
- variables / references
	- alias
		- value reference: let a = 5
			- compiler 'any time I see a in the code, I will replace a with five'
		- expression refference
			- compiler 'wherever I see a, I replace with the value of x'
	- evaluated
		- temporary variable:
- iteration
	- ``` 
	  // Array right fold: computes the following:
	  //   callback(callback(... callback(value, a[n-1]) ..., a[1]), a[0])
	  // Arguments:
	  // value - the initial value.
	  // array - a pointer to an array.
	  // elm_size - the size of an element in the array.
	  // n_elms - the number of elements in the array.
	  // callback - a function pointer to the callback. Expected signature: (felt, T*) -> felt.
	  //
	  // Use starkware.cairo.common.registers.get_label_location() to convert a function label to
	  // a callback value.
	  func array_rfold(value, array: felt*, n_elms, elm_size, callback: felt*) -> (res: felt) {
	      if (n_elms == 0) {
	          return (res=value);
	      }
	  
	      [ap] = value, ap++;
	      [ap] = array, ap++;
	      call abs callback;
	      // [ap - 1] holds the return value of callback.
	      return array_rfold(
	          value=[ap - 1],
	          array=array + elm_size,
	          n_elms=n_elms - 1,
	          elm_size=elm_size,
	          callback=callback,
	      );
	  }
	  ```