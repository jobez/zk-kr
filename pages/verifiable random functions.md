- is
	- a cryptographic primitive that maps inputs to verifiable psuedorandom outputs
	- given an input value x
		- the knowledge of the secret key SK allows one to compute y = F_sk(x) togher with the proof of correctness pi_x.
		- this proof convinces every verifier that
			- the value y = F_sk(x) is indeed correct with respect to
				- the public key of the VRF
		- we can view VRFs as a commitment to a number of random-looking bits
	- the owner of a secret key
		- can compute
			- the function value
		- as well as
			- an associated proof for any input value
	- everyone else
		- using the proof and the associated public key or verification key
			- can check that
				- this value was indeed calculated correctly,
					- yet this information cannot be used to find the secret key
	- such functions are ideal to find block producers in a blockchain in a trustless verifiable way
	-
	-