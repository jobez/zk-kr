- ((641a3a21-429b-4571-8681-05fe2fd4721e))
	- part 1: memoization
	  collapsed:: true
		- kind of like caching the results of calling a function,
			- so when you call it again with the same inputs, you get the result from the cache
		- usually implemented as a higher order-function
			- meaning it takes a function as input and returns another function
			- an idiot implementation of `memoize(x)` in python
				- ```
				  cache = {}
				  def memoize(f):
				      # The memoized version of f(x).
				      def f2(x):
				          if not x in cache:
				              cache[x] = f(x)
				          return cache[x]
				      return f2
				  ```
		- learning: memoization is wraping a function w/ a result cache
	- part 2 zk proofs - a practical walkthrough in cairo
	  collapsed:: true
		- let's recap zk proofs - a shorthand for zk- [[STARK]]s and [[snarks]]
		- [[zero knowledge proofs]] prove computation
			- in such a way that
				- verifying the proof (of computation)
				  id:: 641a3c2a-dd71-4ab5-b96b-3aa6d1aecc7a
					- takes less time than running the computation
					  id:: 641a3c39-b901-4c45-99b5-b04a4b1602ee
		- for a program that takes N computation steps
			- we can
				- prove it
				- verify that proof
			- in
				- O(log^2 N) steps using STARKS
					- an exponential speed up
						- summary of this in https://twitter.com/liamzebedee/status/1516241618919374851
		- what does zk proving look like?
			- usually you write a "provable" program using a language like cairo
				- which
					- compiles to bytecode, which is executed in a VM that generates a [[execution trace]]
						- basically a dump of the values of the VM's [[cpu registers]]
			- ```
			  # Compile.
			  cairo-compile ./program.cairo --output ./program.json
			  
			  # Run.
			  cairo-run --program=program.json --layout=all --memory_file=memory.bin --trace_file=trace.bin
			  # prove
			  giza prove --trace=trace.bin --memory=memory.bin --program=program.json --output=proof.bin
			  # verify
			  giza verify --proof=proof.bin
			  ```
			- the proof is generated as a file `proof.bin`
			- the proving step will take longer than running the program, but the verification will be much shorter
		- how do we know what we're actually verifying
			- obviously it could be a proof of anything
			- in some ZK systems like [[circom]], we have two separate compilation outputs
				- the [[proving circuit]]
				- the [[verification circuit]]
			- the latter of which we would use to authoratively verify a program un
			- in [[StarkWare]]
				- they have a primitive higher up the stack called the [[bootloader]]
					- which is
						- a known program that will execute any program given to it
				- how do we know we're running the bootloader?
					- the [[program hash]]
						- is exposed as
							- public memory input into the program run and because its public memory, we can access it as part of the proof in proof.bin
							- this is what it looks like conceptually
								- ```
								  func bootloader(code: felt*, program_hash: felt, input: felt*) {
								      # communicate with the verifier that we are executing this program.
								      assert hash(code) == program_hash
								      # write program_hash to public_memory
								      
								      # execute
								      push input to memory
								      jump code
								      # equivalent to f(x), where f=code, x=input
								  }
								  ```
							- to illustrate how this works
								- we might have `bootloader.py`
									- which will setup the bootloader to run our program
									- then proving looks the same
										- ```
										  # This will write the program.json and the input data (fib(20)) to the bootloader's memory slot.
										  python3 bootloader.py load --program program.json --entrypoint "fib(20)" --output-memory memory.bin
										  
										  # Now we run using the bootloader, which will run fib(20).
										  cairo-run --program=bootloader.json --layout=all --memory_file=memory.bin --trace_file=trace.bin
										  
										  # Same steps for prove.
										  giza prove --trace=trace.bin --memory=memory.bin --program=program.json --output=proof.bin
										  
										  # Verify.
										  # a. Verify we are running program.json, by checking the program_hash == H(program.json) inside proof.bin.
										  python3 bootloader.py verify-invocation --proof proof.bin --program program.json --entrypoint "fib(20)"
										  # b. Verify the proof.
										  giza verify --proof=proof.bin
										  ```
								- and that's basically the full runthrough of a real zk-stark system, minus the bootloader
								- learning
									- we
										- write
											- a program in Cairo
										- compile program to bytecode
										- run bytecode of program to generate a trace
										- prove the trace
										- verify the proof
									- to verify what we're verifying
										- we use something called a [[bootloader]], which
											- communicates
												- the program hash in the proof data via public memory
										-
	- part 3: recursive zk proofs
	  collapsed:: true
		- so what the fuck is recursive zk proving?
			- there is no call stack that is growing in size
			- there is no calling the same function
			- it is this idea of verifying proofs within proofs
				- basically you have `giza prove`
					- but
						- this program is actually implemented in [[cairo]]
				- so its an api you can call within your ZK programs
					- which
						- allows you to
							- verify proofs from other progrma invocations
				- this is fucking magic
					- because you can verify a proof that verifies other proofs
			- learning:
				- recursive zk proofs are extremely powerful
				- but the recursion occurs conceptually, not like it looks like in normal programming
					- no call stacks
					- no calling the same function
	- part 4: a better analogy for recursive proofs
		- validity proof is kind of like a secure cache
		- if i compute fib(n=100) and give you a number,
			- you can't trust me w/o running the fib(n=100), which is O(N) steps
		- but what if I prove fib(n=100) and give you the proof?
			- then it costs you 0(log^2N) steps to trust me, by verifying the proof
		- what might this look applied to our memoize helper?
			- ```
			  cache = {}
			  def memoize(f):
			      # The "securely" memoized version of f(x).
			      def f2(x):
			          if not x in cache:
			              cache[x] = prove(f(x))
			          return verify(cache[x])
			      return f2
			  ```
		- learning: another way to understand recursive proof is through the idea of a secure cache
			- the cache values are checked automatically using the `verify` function from  Starkware
	- part 5: a better api for recursive proofs
		- annotate any function by provable
		- built in runner allows a shared cache for all function invocations
		- the runtime would aotmatically memoize and check the cache
		- here's how this might work
			- a public area of memory dedicated to the cache
			- cairo-run and gize are modified to accept `secure cache entries` - proofs
			- the cairo runtime automatically writes functions annotated as provable to check this cache for results, thus removing the need for the programmer to write 'verify'