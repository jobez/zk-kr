- cairo, cpu, fields
	- Are you familiar with starknet's language Cairo? I want to build more intuition for its value proposition.
	- A prover, in order to generate a validity proof, needs the [[execution trace]] of the computation performed.
		- ((64134b53-9043-48f2-9dc2-dd3b8e80d8a8))
	- This means that the nature of the computation done in cairo is different than non 'provable' programs, right?
	- The quote I have is that in cairo programs, you write what results are acceptable, not how to come up with results. You run your logic /and/ assert that this has been done correctly.
		- ((641354b8-bcc3-42bb-8946-520c97786410))
		- In a Cairo program, you describe the desired computation and provide constraints that the computation should satisfy.
			- This is different from typical programming languages where you only provide a sequence of instructions to obtain a result.
			- Cairo allows you to express both the computation and the constraints that ensure the correctness of the computation.
	- So cairo uses a STARK to represent a trace of an execution of a cairo machine. This trace is a table of values in a polynomial--the trace being 'broken down' computation at the level of the cpu, which has register and memory transitions. We take those transitions as a table of values in a polynomial.
		- The value for each memory cell in the cairo runtime is chosen by the prover and cannot be changed during execution. So its memory is immutable.
		- Cairo programs specify constraints that the computation must satisfy.
		- These constraints define what makes [[a valid execution trace]].
		- The [[prover]] demonstrates that [[the given execution trace]] satisfies these constraints, essentially proving that the program has been executed correctly.
	- Ok, so let's try to take a step back and reason about the mapping of cpu transitions to execution trace by considering the 'implementation details' of cairo that have it using field integers.
		- ((635a9851-a634-478f-aeb8-cce629856498))
		- We are essentially taking a model of a cpu and running it in a system that 'wants' to convey structure in a field bounded by some large prime number. Why?
			- The reason Cairo uses field arithmetic and operates within a large prime field is that it relies on algebraic techniques, such as [[polynomial]] operations and evaluations, to create zk- [[STARK]] proofs. These techniques are most efficient and secure when performed in a [[finite field.]]
				- ((635a9851-7da4-41d2-83c7-40eb2bf8353e))
			- [[finite field]] s, also known as Galois fields, have a finite number of elements and well-defined arithmetic operations. A large prime field is a finite field with a prime number of elements. In the context of Cairo and zk-STARKs, using a large prime field offers several advantages:
				- Algebraic structure: Finite fields provide a rich algebraic structure that supports addition, subtraction, multiplication, and division (except by zero) with well-defined properties. This structure enables the use of polynomial interpolation, evaluation, and other algebraic techniques in creating and verifying zk-STARK proofs.
				- Computational efficiency: Arithmetic operations in a large prime field can be performed efficiently using modular arithmetic.
				- [[modular arithmetic with prime moduli]] exhibits desirable properties, such as the existence of unique inverses for non-zero elements, which simplifies calculations and makes it possible to create compact and efficient proofs.
				- Security: Working within a large prime field provides [[a natural source of hardness for cryptographic assumptions]].
				- Many cryptographic primitives rely on [[the difficulty of solving certain problems in a finite field]], such as the [[Discrete Logarithm Problem]] (DLP).
					- This hardness is essential for the security of zk-STARKs and other cryptographic constructions.
					- ((64131b44-266a-4a99-a566-eb9041eaeec3))
				- Error detection: Performing arithmetic in a finite field helps detect errors in the computation. For example, if an intermediate result of an operation in a Cairo program is incorrect, it is more likely to result in [[an invalid field element,]] making it easier to identify [[incorrect execution traces]] .
-