- practice using [[zokrates]]
	- ((64131f56-d48a-49c5-b47f-d11c89b764a5))
	- use example file to generate a [[proof]]
		- to show that
			- a [[prover]] knows the square root of 25
	- try to create an invalid proof
	- follow the example
	  collapsed:: true
		- to build
			- a proof that you know the pre-image of a hash
				- https://zokrates.github.io/examples/sha256example.html
			- We'll implement an operation that's very typical in blockchain use-cases: proving knowledge of the preimage for a given hash digest. In particular, we'll show how ZoKrates and the Ethereum blockchain can be used to allow a prover, let's call her Peggy, to demonstrate beyond any reasonable doubt to a verifier, let's call him Victor, that she knows a hash preimage for a digest chosen by Victor, without revealing what the preimage is.
			- We will start this tutorial by using ZoKrates to compute the hash for an arbitrarily chosen preimage, being the number `5` in this example.
			- `sha256packed` is a SHA256 implementation that is optimized for the use in the ZoKrates DSL. Here is how it works:
				- We want to pass 512 bits of input to SHA256. However, a `field` value can only hold 254 bits due to the size of the underlying prime field we are using.
				- As a consequence, we use four field elements, each one encoding 128 bits, to represent our input.
				- The four elements are then concatenated in ZoKrates and passed to SHA256. Given that the resulting hash is 256 bit long, we split it in two and return each value as a 128 bit number.
			- Let's recall our goal: Peggy wants to prove that she knows a preimage for a digest chosen by Victor, without revealing what the preimage is. Without loss of generality, let's now assume that Victor chooses the digest to be the one we found in our example above.
			  id:: 64132336-a756-4570-bb4e-b9f8e661ccc8
				- To make it work, the two parties have to follow their roles in the protocol:
					- First, Victor has to specify what hash he is interested in. Therefore, we have to adjust the zkSNARK circuit, compiled by ZoKrates, such that in addition to computing the digest, it also validates it against the digest of interest, provided by Victor. This leads to the following update for `hashexample.zok`:
						- Note that we now compare the result of `sha256packed` with the hard-coded correct solution defined by Victor. The lines which we added are treated as assertions: the verifier will not accept a proof where these constraints were not satisfied. Clearly, this program only returns 1 if all of the computed bits are equal.
					- So, having defined the program, Victor is now ready to compile the code:
						- setup creates a verification key and a proving key
						- victor gives a proving key to peggy
						- export verifier creates a verifier.sol contract that contains our verification key and a function `verifyTx`
						- victor deploys this smart contract to the ethereum network
						- peggy provides the correct pre-image as an argument to the program
						- finally, peggy can run the command to construct the proof
							- As the inputs were declared as private in the program, they do not appear in the proof thanks to the zero-knowledge property of the protocol.
							- zokrates creates a file `proof.json`
								- consisting of
									- three elliptic curve points
										- that make up
											- the [[snarks]] proof
							- the `verifyTx` function in the smart contract deployed by Victor accepts these three values, along with an array of public inputs
								- the array of public inputs consists of
									- any public inputs to the main function, declared without the `private`
									- the return values of the zokrates function
							- in the example we're considering
								- all inputs are private and there is a single return vlaue of 1
	- in principle how could you use zokrates
		- to verify that
			- a certain address on Ethereum has more than say 1 eth?
		- high level: you need to use a combo of zokrates and an oracle that provides ethereum account  balance information
		  collapsed:: true
			- set up an oracle service that provides account data
				- this service should be able to return the account balance for a specific address at a given block number
			- create a zk-snark circuit using zokrates
				- the circuit should take the account balance as private input and generate a proof that the balance is greater than 1 ETH w/o revealing actual balance
			- compile and setup the zokrates circuit
				- write a zokrates DSL code for the circuit
				- compile it
				- and generate the necessary [[proving key]] and [[verifying key]]
				- integrate the oracle and the zk-snark circuit
					- connect the oracle service to the zokrates circuit
						- so that
							- the circuit can acccess the account balance at a specific block number
							- the oracle should provide the account balance as a private input to the zokrates circuit
				- generate a zk-snark proof
					- using zokrates toolbox to generate a proof that demonstrates the account balance is greater than 1 eth, without revealing the actual balance
			-
		- high level: you need to use a combo of zokrates and a snark circuit that veerifies the correctness of a storage proof and that the balance is greather than 1 eth
			- design a circuit that takes the following inputs
				- ethereum address and the associated account balance (private inputs)
				- the storage proof, including the merkle path from the state trie root to the account balance (as public inputs)
				- the circuit should verify the correctness of the storage proof and check that the balance is greater than 1 eth
			- compile and set up the zokrates circuit
			- generate the storage proof
				- for a specific block
					- extract the storage proof for the account balance of the address in question
					- generate a zk-snark proof
					- deploy verifier on ethereum