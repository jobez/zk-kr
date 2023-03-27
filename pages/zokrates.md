- the creator writes and compiles a program in the zokrates dsl
  collapsed:: true
	- the creator / prover generates a trusted setup for the compiled program
	- the prover computes a [[witness]] for the compiled program
	- the prover generates a proof
		- using the proving key
			- she generates a proof for
				- a computation of the compiled program
	- the creator/prover exports a verifier
		- using the verifying key
			- she generates a Solidity contract which contains
				- the generated verification key and a public function
					- to verify
						- a solution to the compiled program
- the program checks if the prover knows what it claims to know
  collapsed:: true
	- C creates proof
	- verifier in solidity consumes proof and returns yes / or
- parameters of system
  collapsed:: true
	- scheme
	- curve
- https://blog.decentriq.com/proving-hash-pre-image-zksnarks-zokrates/
	- implements a problem very typical for blockchain use-cases:
		- proving the knowledge of a pre-image for a given sha-256 digest
	- we begin demystifying this machinery by computing the [[SHA-256]] hash of the number 5
	- we will show how zokrates and the ethereum blockchain
		- can be used to allow
			- a [[prover]] (Alice)
				- to demonstrate beyond any reasonable doubt to
					- a [[verifier]]
				- that
					- she knows a hash of a pre-image for a chosen digest by verifier without revealing what the pre-image is
	- prereqs
		- collapsed:: true
		  ```
		  import hashlib  
		  
		  preimage = bytes.fromhex('00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00\
		  00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 05')
		  
		  bin(int(preimage.hex(), 16)) //binary representation of pre-image 
		  //output is '0b101'
		  
		  hashlib.sha256(preimage).hexdigest() //compute hash
		  //output is
		  //'c6481e22c5ff4164af680b8cfaa5e8ed3120eeff89c4f307c4a6faaae059ce10'
		  ```
			- we define a 512 bit pre-image using a hexadecimal notation and assign the resulting byte object to the variable preimage
		- in zokrates
		  collapsed:: true
			- first we create a new file named `hashexample.code` with the following content
			  collapsed:: true
				- ```
				  import "LIBSNARK/sha256" as sha256
				  
				  def main() -> (field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field, field):
				  	o255, o254, o253, o252, o251, o250, o249, o248, o247, o246, o245, o244, o243, o242, o241, o240, o239, o238, o237, o236, o235, o234, o233, o232, o231, o230, o229, o228, o227, o226, o225, o224, o223, o222, o221, o220, o219, o218, o217, o216, o215, o214, o213, o212, o211, o210, o209, o208, o207, o206, o205, o204, o203, o202, o201, o200, o199, o198, o197, o196, o195, o194, o193, o192, o191, o190, o189, o188, o187, o186, o185, o184, o183, o182, o181, o180, o179, o178, o177, o176, o175, o174, o173, o172, o171, o170, o169, o168, o167, o166, o165, o164, o163, o162, o161, o160, o159, o158, o157, o156, o155, o154, o153, o152, o151, o150, o149, o148, o147, o146, o145, o144, o143, o142, o141, o140, o139, o138, o137, o136, o135, o134, o133, o132, o131, o130, o129, o128, o127, o126, o125, o124, o123, o122, o121, o120, o119, o118, o117, o116, o115, o114, o113, o112, o111, o110, o109, o108, o107, o106, o105, o104, o103, o102, o101, o100, o99, o98, o97, o96, o95, o94, o93, o92, o91, o90, o89, o88, o87, o86, o85, o84, o83, o82, o81, o80, o79, o78, o77, o76, o75, o74, o73, o72, o71, o70, o69, o68, o67, o66, o65, o64, o63, o62, o61, o60, o59, o58, o57, o56, o55, o54, o53, o52, o51, o50, o49, o48, o47, o46, o45, o44, o43, o42, o41, o40, o39, o38, o37, o36, o35, o34, o33, o32, o31, o30, o29, o28, o27, o26, o25, o24, o23, o22, o21, o20, o19, o18, o17, o16, o15, o14, o13, o12, o11, o10, o9, o8, o7, o6, o5, o4, o3, o2, o1, o0 = sha256(0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,1)
				  	return o255, o254, o253, o252, o251, o250, o249, o248, o247, o246, o245, o244, o243, o242, o241, o240, o239, o238, o237, o236, o235, o234, o233, o232, o231, o230, o229, o228, o227, o226, o225, o224, o223, o222, o221, o220, o219, o218, o217, o216, o215, o214, o213, o212, o211, o210, o209, o208, o207, o206, o205, o204, o203, o202, o201, o200, o199, o198, o197, o196, o195, o194, o193, o192, o191, o190, o189, o188, o187, o186, o185, o184, o183, o182, o181, o180, o179, o178, o177, o176, o175, o174, o173, o172, o171, o170, o169, o168, o167, o166, o165, o164, o163, o162, o161, o160, o159, o158, o157, o156, o155, o154, o153, o152, o151, o150, o149, o148, o147, o146, o145, o144, o143, o142, o141, o140, o139, o138, o137, o136, o135, o134, o133, o132, o131, o130, o129, o128, o127, o126, o125, o124, o123, o122, o121, o120, o119, o118, o117, o116, o115, o114, o113, o112, o111, o110, o109, o108, o107, o106, o105, o104, o103, o102, o101, o100, o99, o98, o97, o96, o95, o94, o93, o92, o91, o90, o89, o88, o87, o86, o85, o84, o83, o82, o81, o80, o79, o78, o77, o76, o75, o74, o73, o72, o71, o70, o69, o68, o67, o66, o65, o64, o63, o62, o61, o60, o59, o58, o57, o56, o55, o54, o53, o52, o51, o50, o49, o48, o47, o46, o45, o44, o43, o42, o41, o40, o39, o38, o37, o36, o35, o34, o33, o32, o31, o30, o29, o28, o27, o26, o25, o24, o23, o22, o21, o20, o19, o18, o17, o16, o15, o14, o13, o12, o11, o10, o9, o8, o7, o6, o5, o4, o3, o2, o1, o0
				  
				  ```
				- main does not take any input arguments
				- the defined program is just calling the imported `sha256` function using 512 field elements, each one representing one bit of the input
				- unsurprisingly, we set the input values to our pre-image, i.e. 5, using the same binary encoding we derived above
				- in addition, we declare 256 field elements and set them to the output of the hash function
				- the last line defines the return statement for all these elements, i.e. the computed digest
				- having our problem described in zokrates' DSL, we can continue using zokrates for the rest of our workflow
					- first we compile the program into an arithmetic [[circuit]]
						- `zokrates compile -i hashexample.code`
					- as a next step we can create a witness file using the following command
						- `zokrates compute-witness`
							- note that the -a flag is not used in this case as we do not have to specify any public input argument for this argument
							- at this point we can compare the created witness with the results obtained in python
								- `
								  grep -r '~out_' witness | awk '{split($0,a,"out_"); print a[2]}' | sort -k1 -n | head -n 25 |  awk -F ' ' '{print $2}' | tr  -d '\n'
								  `
									- which results in the following output
										- `1100011001001000000111100`
										- recall the 25 most significant bits of the hash digest obtained in the python section are equivalent
				-
	- prove knowledge of pre-image
	  collapsed:: true
		- for now we have seen that
			- besides computing our hash in python
			- we can compute it using heavy finite field arithmetic on [[Elliptic Curves]] using zokrates
			- aswelas on the global state machine [[Ethereum]] using solidity
		- let's recall our goal
			- a prover wants to prove that she knows a pre-image for a digest chosen by Victor, without revealing what the pre-image is
			- without the loss of generality, let's now assume that victor choses the digest to be equivalent to our example above
			- to make it work, the two parties have to follow their roles in the protocol
			- first, victor has to specify what hash digest he is interested in
				- therefore, we have to adjust the zksnark circuit such that in addition to computing the digest, it also validates it against the digest of interest, provided by victor
				- this leads to the following update for `hashexample.code`
					- define main function to accept 512 private input arguments
					- these inputs are used to encode the secret-primage for which to compute the digest
					- we must compare the result bit by bit with the hard-coded correct solution defined by victor
			- `zokrates setup`
				- will create a verification.key and a proving.key file
				- victor must make the proving.key publicly available to Alice
			- `zokrates export-verifier`
				- creates a `verifier.sol` solidity contract that contains our verification key and a nifty function `verifyTx`
			- alice uses the `proving.key` to compute a [[witness]] to the problem
				- ```
				  ./zokrates compute-witness -a 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 1
				  ```
					- note that alice now provides the correct pre-image as an argument to the
					- however, these inputs are declared as private variables in the program and don't leak to victor due to the zk aspect of t he protocol
			- finally, alice can run the command to construct the proof
				- `./zokrates generate-proof`
					- zokrates creates a file for the proof consisting of the eight varaibles that make up the [[snarks]] proof
					- the `verifyTx` function in the smart contract deployed by victor accepts these eight values, along with an array of public input
						- the array of public inputs consists of
							- any parameters in the zokrates function, which do not contain the private keyword
						- the return values of the zokrates function
				- in the example we're considering, all inputs are private and there is a single return value of 1, hence alice has to define her public input array as follows
				- victor is continuously monitoring the verification smart ocntract for the `Verified` event, which is emitted upon succesful verification of a transaction
					- as soon as he observes the event triggered by a transaction from Alice's public address, he can be sure that Alice has a valid pre-image for the SHA-256 digest he has put into the smart contract
			-
	- summary