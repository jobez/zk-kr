- general description
  collapsed:: true
	- general computation on a blockchain allow a different relationship between results of interactions of stakeholders and  institutional oversight
	- this different relationship that seeks to lessen the demand for 'fiat' institutional oversight to establish the legitimacy of results of interactions of stakeholders leads to a scaling problem.
	- the scaling problem arises because the attempt to minimize institutional oversight leads to the verification of the results of interaction to be done across a permissionless class of participants, where each participant validates a computation.
	- [[interactive proof protocols]] allow a different approach to verification of computational claims, one that is more efficient.
	- cairo uses a method a particular cyptographic method, [[STARK]], to verify claims about the execution in one particular Turing complete model of computation, the [[Cairo machine]]
	- cairo uses STARK to represent a trace of an execution of a CAIRO machine as a table of values in a polynomial via an encoding the claims of an execution of a CAIRO program to an [[Algebraic Intermediate Representation]]
		- the claim that a program has executed successfully is expressed as a claim about the existence of a solution to a family of [[polynomial]]s
- memory model
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