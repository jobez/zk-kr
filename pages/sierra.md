- https://medium.com/yagi-fi/reading-sierra-starknets-secret-sauce-for-cairo-1-0-5bc73409e43c
	- a stable layer to allow for rapid iteration and innovation at the Cairo 1.0 language level
	- reading Sierra code can help us get a deeper understanding of the underlying type system, security risks and improving efficiency where it matters (e.g., reducing the storage footprint or execution steps in highly recursive functions)
	- For Cairo 0.x programmers, Sierra is the bridge between our past and the future, a familiar blueprint guiding our craft. Let's take a look.
	- The Sierra output is organized into 4 separate sections always presented in the same order.
		- First come the type declarations.
		- Next, declarations of built-in library functions that will be used.
		- Then the sequence of statements and finally the declared Cairo functions.
	- A couple of observations:
		- The Unit type `()` is a special case of an empty struct and is used as a default return type for procedures (functions without a declared return type)
		- New temporary variables are created and indexed using square brackets ( `[0]` and `[1]` )
		- Statements have the following form `<libfunc>(<inputs>) -> (<outputs>);` , i.e., we don't see the more traditional `[0] = struct_construct<Unit>();`
		- Code blocks are separate from Cairo function declarations. A function is tied to a specific block of code by starting at a dedicated statement index location (note the `do_nothing@0` which indicates that the function begins at the first statement).
- https://medium.com/nethermind-eth/under-the-hood-of-cairo-1-0-exploring-sierra-7f32808421f5
	- two conference talks at Starkware’s Sessions about Sierra
		- [Enforcing Safety Using Typesystems](https://www.youtube.com/watch?v=-EHwaQuPuAASierra) by Shahar Papini
		- [Not Stopping at the Halting Problem](https://www.youtube.com/watch?v=wYxUedcdVZ4) by Ori Ziv
	- The following post is the first of a series in which I’ll dive into Sierra to understand more about Cairo, its mechanisms, and Starknet overall.
		- sierra is an intermediate layer between the high-level language [[cairo]] and the compilation targets such as [[casm]]
		- the language is designed to ensure safety and prevent runtime errors
		- it guarnatees that
			- every function returns
			- that there are no infinite loops by using a compiler that can detect operations that could fail at compile time
		- uses a simple yet powerful typesystem to express mid-level code while ensuring safety
		- this allows for efficient compilation down to CASM
	- motivation
	  collapsed:: true
		- In [[cairo/0]] ,
		  collapsed:: true
			- developers
				- would
					- write Starknet contracts in Cairo,
					- compile them to CASM,
					- and deploy the compilation output directly on Starknet.
				- Users
					- could then interact with Starknet contracts by
						- calling smart contract functions,
						- signing a transaction, and
						- sending it to the [[sequencer]] .
							- The sequencer
								- would
									- run the transaction to get the user’s transaction fee,
								- the [[prover]] ([SHARP](https://www.cairo-lang.org/docs/sharp.html)) [[SHARP]]
									- would generate
										- the proof for the batch including this transaction,
								- and the [[sequencer]] would charge a fee for including the transaction in a block.
		- this flow has a few problems
			- only valid statements can be proven in cairo, so failed txns cannot get proven
				- it is impossible to prove an invalid statement like `assert 0 = 1`
					- translates to [[polynomial constraints]] that can't be satisfied
			- transaction execution may fail
				- resulting in
					- the [[transaction]] not being included in a [[block]]
						- in this case, the sequencer does free work
							- since failed txns don't have valid proofs, they can't be included
							- there is no way to enfore fee transfer for the quencer
			- the sequencer can be subjected to DDoS attacks with invalid transactions
				- causing them to work for nothing and not collect any fee for running these transactions
			- it is impossible to distinguish between [[censorship]]
				- when a [[sequencer]] purposefully decides not to includ ecertain transactions
			- and
				- invalid transactions since both types of transactions will not be included in blocks
			- in ethereum
				- all failed txns are marked as reverted but are still included in blocks,
					- allowing [[validators]] to collect transaction fees upon failure
				- To prevent malicious users from bombarding the network with invalid transactions and overwhelming the sequencers,
					- halting the network as legitimate transactions cannot be processed,
					- Starknet needs a similar system that allows sequencers to collect fees for failed transactions.
					- To solve the issues raised above, the Starknet network needs to achieve two goals: completeness and soundness.
						- [[completeness]] ensures that transaction execution can always be proven, even when it is expected to fail.
						- [[soundness]] ensures that valid transactions are not rejected, preventing censorship.
			- sierra is correct-by-construction
				- letting the [[sequencer]] charge a fee for all transactions
				- instead of deploying code that can fail `asserts`
					- we can deploy branching code `if/else`
					- [[cairo/1]]'s asserts are translated to branching sierra code
						- allowing errors to be propagated back to the original entry point that returns a boolean value of `true/false` for transaction success or failure
						- the [[starknet/os]] can determine if a transaction is valid based on the entry point return value, and decide whether or not to apply the state update if the transaction was successful
			- [[cairo/1]] offers a syntax similar to Rust and abstracts the safety constructs of Sierra to create a provable, developer-friendly programming language
				- it compiles to [[sierra]]
					- a correct-by-construction intermediate representation of Cairo code that doesn't contain any failing semantics
						- this ensures that
							- no sierra code can ever fail, and it compiles down to a safe subset of [[casm]]
					- devs can focus on writing efficient smart contracts without worrying about writing non-failing code, all with i proved security primitives
			- instead of deploying casm code to starknet
				- developers will compile their cairo 1 code to sierra and deploy sierra programs to starknet
				- on a delcare transaction, the sequencers will be responsible for compiling the Sierra code to CASM, ensuring that no failing code can be deployed on starknet
		-
			-
				-
	- correct-by-construction
		- to design a language that cannot fail
			- we must identify the unsafe operations in [[cairo/0]]
			- these include
				- illegal memory address references; attemptig to access an unallocated memory cell
				  collapsed:: true
					- in [[cairo/0]]
						- developers could write the following code that tried to access the content of an unallocated memory cell
						- ```cairo
						  let (ptr: felt*) = alloc();
						  tempvar x = [ptr];
						  ```
						- sierra's type system prevents common pointer-related errors by enforcing strict ownership rules and making use of smart pointers like `Box`
							- thereby enabling
								- the detection and prevention of invalid point dereferencing at compile time
							- the `Box<T>` type serves as a pointer to a valid and initilaized pointer instance and provides two functions for instantiating and dereferencing
								- `box_new<T>()` and `box_deref()`
							- by using the type system to catch dereferencing errors at compile time, 
							  id:: 6421d9bb-ea7d-45a6-b660-c86efaf35c7a
								- [[casm]]
									- compiled from
										- [[sierra]]
									- avoids
										- invalid pointer dereferencing
				- assertions, since they can fail with no way to recover
				  collapsed:: true
					- Assertions are usually employed to assess the result of a boolean expression at a particular point in the code.
					- If the evaluation is not as anticipated, an error is raised. Unlike in [[cairo/0]] the compilation of a [[cairo/1]]  `assert` instruction will produce branching Sierra code.
					- This code will terminate the current function execution prematurely if the assertion is not satisfied and continue with the next instructions otherwise.
					- Dictionaries suffer from the same multiple append problem as Arrays, which can be solved by introducing a special `Dict<K,V>` type and a set of utility functions to instantiate, retrieve, and set dictionary values.
					- However, there is a soundness **issue with dictionaries. Every Dict must be **squashed** at the end of a program by calling the `dict_squash(Dict<K,V>) -> ()` function, which verifies the consistency of the sequence of key updates.
					- Unsquashed dictionaries are dangerous, as a malicious prover could prove the correctness of inconsistent updates.
					- As we previously saw, the linear type system enforces objects to be used exactly once.
					- The only way to “use” a Dict is to call the `dict_squash` function, which uses the dictionaries instance and returns nothing.
					- This means that unsquashed dictionaries will be detected when compiling Sierra code to [[casm]] , and an error will be thrown at compile time.
					- For other types that don't necessarily need to be used exactly once, usually types that don't contain Dicts, Sierra introduces the `drop<T>(T)->()` function that uses an instance of an object and doesn't return anything.
					- It’s worth noting that both `drop` and `dup` don't produce any CASM code. They simply provide type safety at the Sierra level, ensuring that variables are used exactly once.
				- multiple writes to the same memory address due to Cairo's write once memory model
				  collapsed:: true
					- in [[cairo/0]]
						- users would use arrays like this
							- ```
							  let (array:felt*) = alloc();
							  assert array[0] = 1;
							  assert array[1] = 2;
							  assert array[1] = 3; // fails
							  ```
						- however, attempting to write twice to the same array index
							- results in
								- a runtime error as memory cells
									- can only be
										- written once
						- However, attempting to write twice to the same array index results in a runtime error as memory cells can only be written once.
						- To avoid this issue, Sierra introduces an `Array<T>` type along with an `array_append<T>(Array<T>, value:T) -> Array<T>` function.
						- This function takes an instance of an array and a value to append and returns the updated array instance pointing to the new next free memory cell.
						- As a result, values are sequentially appended to the end of arrays without worrying about possible clashes due to already-written memory cells.
						- To ensure that previously used array instances that have already been appended are not reused, Sierra uses a **linear type system** that ensures that objects are used **exactly once**.
						- As such, any Array instance that has already been appended cannot be reused in another `array_append` call.
						- The code below shows a snippet from a Sierra program that creates a felt array and appends the value `1` twice using the `array_append` libfunc.
						- In the code, the first `array_append` call takes the array variable with id `[0]` as input and returns a variable with id `[2]` representing the updated array.
						- This variable is then used as an input parameter for the next `array_append` call.
						- It is important to note that the variable with id `[0]` is not reusable once consumed by the libfunc, and attempting to invoke `array_append` with `[0]` as an input parameter will result in a compilation error.
						- ```
						  array_new<felt>() -> ([0]);
						  felt_const<1>() -> ([1]);
						  store_temp<felt>([1]) -> ([1]);
						  array_append<felt>([0], [1]) -> ([2]);
						  felt_const<1>() -> ([4]);
						  store_temp<felt>([4]) -> ([4]);
						  array_append<felt>([2], [4]) -> ([5]);
						  ```
						- For objects that can be reused multiple times, such as `felts`, Sierra provides the `dup<T>(T) -> (T,T)` function, which returns two instances of the same object that can be used for different operations.
						- This function is only implemented for types that are safe to duplicate, typically types that do not contain arrays or dictionaries.
				- infinte loops which make it impossible to determine whether a program will exit
					- Determining whether a program will eventually halt or run forever is a fundamental problem in computer science known as the Halting Problem, and unfortunately, it is unsolvable in the general case.
					- In a distributed environment like Starknet, where users can deploy and run arbitrary code, it is important to prevent users from running infinite loops such as the following Cairo code.
					- As recursive functions can be a source of infinite loops if the stop condition is never met, the Cairo-to-Sierra compiler will inject a `withdraw_gas` method at the beginning of recursive functions.
					- As this feature has not yet been implemented, developers are still expected to call `withdraw_gas` in recursive functions and handle the result themselves, although it should be included in a future release of the compiler.
					- This `withdraw_gas` function will deduct the amount of gas required to run the function from the total gas available for the transaction by evaluating the cost of running each instruction in the function.
					- The cost is analyzed by determining how many steps each operation takes - which is known at compile time for most operations.
					- During the execution of a Cairo program, if the `withdraw_gas` call returns a null or negative value, the current function execution will stop, all pending variables will be consumed by calling `dict_squash` on unsquashed dictionaries and calling `drop` on other variables, and the execution will be considered as failed.
					- Because all transactions on Starknet have a limited amount of spendable gas to execute the transactions, infinite loops are avoided - and by ensuring that enough gas is still available to drop the variables and stop the execution, the sequencer will be able to collect the fees from the transaction failure.