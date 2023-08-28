- the most common proof system being used
	- for example
		- they form the basis for the privacy provided in [[zcash]]
- the process of creating and using a zk-snark can be summarised as
	- a zk-snark consists of three algorithims: C, P, V
		- defined as follows
			- The Creator takes a secret parameter lambda and a program C and generates two publicly available keys
				- a proving key pk
				- a verification key vk
			- these keys are public parameters that only need to be generated once for a given program C. they are also known as the [[Common Reference String]]
			- the prover peggy takes a proving key, a public input x and a private witness w.
				- peggy generates a proof pr = P(pk, x, w) that clains that peggy knows a witness w and that the wtiness satisfies the program C
			- the verifier Victor computes V(vk, x, pr) which returns true if the proof is correct and false otherwise
			- thus this function returns true if Peggy knows a witness w satisfying
				- C(x,w) = true
- trusted setups and toxic waste
  collapsed:: true
	- note the secret parameter lambda in the setup
	- this parameter sometimes make it tricky to use zk-SNARK in real world applications
	- the reason for this is that
		- anyone who knows this parameter can generate fake proofs
	- specifically, given any program C and public input x
		- a person who knows lambda can generate a proof pr2 such that V(vk, x, pr2) evaluates to truw tihought knowledge of the secret w
- interactive v non interactive proofs
  collapsed:: true
	- non-interactivity is only useful if
		- we want to allow multiple independent verifiers to verify a given proof without each one having to individually query the prover
	- in contrast, in non-interactive zero-knowledge protocols there is no repeated communication between the prover and the verifier
	- instead, there is only a single round, which can be carried out asynchronously
- succinct v non succinct
  collapsed:: true
	- succinctness is necessary only if
		- the medium used for storing the proofs is very expensive and/or if we need very short verification times
- proof v proof of knowledge
  collapsed:: true
	- a proof of knowledge is stronger and more useful than
		- just proving the statement is true
	- for instance, it alows me to prove that I know a secret key, rather than just that it exists
- ```mermaid
  t/4