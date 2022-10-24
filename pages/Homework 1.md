- #+BEGIN_QUOTE
  In your teams, think of some use cases of [[zero knowledge proofs]]
  What problems are there when using zkps in real world situations, such as providing an
  identity application for use in airports.
  #+END_QUOTE
	- The 'what' of a zero knowledge proof is a formalism that describes an interaction between a prover and a verifier. That description of an interaction is probablistic, and the guarantee that the formalism is making is that the prover can't fool the verifier within an interaction with computational constraints.
	- What that formalism offers is a way to handle the concerns of interaction / games / civility where a 'prover' (the role of an agent wanting to make a certain claim) and 'verifier' (the role of an agent wanting to verify the legitimacy of that claim) can get what they want without the verifier needing the prover to disclose secrets
	- How it does this involves field arithmetic, which is a set of numbers that is a group over arithmetic and multiplication up to its 'order', which is a prime number amount of the numbers in that set.
	- So one use case where civil systems organize around claims that can involve the compromising of secrets are things that revolve around credit scores. We want to be able to claim some trend in financial transactions of users without disclosing the exact nature of said users transactions.
- #+BEGIN_QUOTE
  Working with the following set of Integers S = {0,1,2,3,4,5,6,7,8}
  What is
  a) 4 + 5
  b) 3 x 5
  #+END_QUOTE
	- a) 4 + 5 = 9 mod 9 = 0
	- b) 3 x 5 = 15 mod 9 = 6
- #+BEGIN_QUOTE
   For S = {0,1,2,3,4,5,6,7,8}
  a) Can we consider 'S' and the operation multiplication to be a group ?
  b) Can we consider 'S' and the operation division to be a group ?
  #+END_QUOTE
	- Because multiplication is associative (binary operation can be done in any order of elements given) and commutative (the order of elements can be rearranged before applying the binary operation) and has an identity element (1), we can consider S a group with respect to multiplication.'
	- Since division is not associative or commutative, S is not considered a group with respect to division. No cap.
	-