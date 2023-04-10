- [[snarks]] is the truly ingenious method of proving that something is true without revealing any other information
  collapsed:: true
	- why is this useful in the first place?
		- proving a [[statement]] on [[private data]]
			- person A has more than X in his bank account
			- in the last year, a bank did not transact with an entity Y
			- matching DNA without revealing full DNA
			- one has a credit score higher than Z
		- anonymous authorization
			- proving that requester R has right to access web-sites restricted area without revealing its identity
			- prove that one is from the list of allowed countries/states without revealing from which one exactly
			- prove that one owns a monthly pass to a subway/metro without revealing card's id
		- anonymous payment
			- payment with full detatchment from any kind of identity
			- paying taxes without revealing one's earnings
		- outsourcing computation
			- [[validity proof]] / [[validity rollup]]
- diagrams
  collapsed:: true
	- id:: 642a0300-5d51-43a9-928e-f0158b6d47ad
	  ```mermaid
	  graph LR
	    Prover -->|Convince| Protocol
	    Verifier -->|Learn| Protocol
	    Protocol -->|Satisfy| Properties
	    Properties --> Completeness
	    Properties --> Soundness
	    Properties --> ZeroKnowledge
	  
	    classDef goals fill:#FFA726,stroke:#333,stroke-width:2px;
	    classDef properties fill:#4CAF50,stroke:#333,stroke-width:2px;
	    class Prover,Verifier goals;
	    class Completeness,Soundness,ZeroKnowledge properties;
	  
	    subgraph Completeness
	      compDef1("if the statement is true then a prover can convince a verifier")
	    end
	  
	    subgraph Soundness
	      sndDef1("a cheating prover can not convince a verifier of a false statement")
	    end
	  
	    subgraph ZeroKnowledge
	      zkDef1("the interaction only reveals if a statement is true and nothing else")
	    end
	  
	  
	  ``` #gpt/4
	- ```mermaid
	  graph LR
	    A[Polynomial Properties] --> B[Unique Identity]
	    A --> C[Intersect at most d points]
	    B --> D[Efficient Comparison]
	    C --> D
	    D --> E[zk-SNARKs Proof Systems]
	  
	    classDef properties fill:#FFA726,stroke:#333,stroke-width:2px;
	    classDef conclusion fill:#4CAF50,stroke:#333,stroke-width:2px;
	    class A properties;
	    class B,C,D properties;
	    class E conclusion;
	  
	  ```
		- {{embed ((642a0d3e-2dc0-4f4d-be82-9673213f6674))}}
	- collapsed:: true
	  ```mermaid
	  graph LR
	    A[Byte Array] --> B[O of n Time Complexity]
	    A --> C[Inefficient for Large Arrays]
	    D[Polynomial] --> E[Unique Identity]
	    D --> F[Intersect at most d points]
	    D --> G[Efficient Comparison]
	    B --> H[Less Suitable for zk-SNARKs]
	    C --> H
	    E --> I[More Suitable for zk-SNARKs]
	    F --> I
	    G --> I
	  
	    classDef medium fill:#FFA726,stroke:#333,stroke-width:2px;
	    classDef property fill:#4CAF50,stroke:#333,stroke-width:2px;
	    classDef conclusion fill:#2196F3,stroke:#333,stroke-width:2px;
	    class A,D medium;
	    class B,C,E,F,G property;
	    class H,I conclusion;
	  
	  ``` #gpt/4
		- ((642a10a0-ccb0-4395-b78d-449ec026c27f))
	- ```mermaid
	  graph LR
	    A[Fundamental Theorem of Algebra] --> B[Prove Prover Knows Polynomial]
	    B --> C[Protocol for Polynomial Identity Checks]
	    C --> D[Verifier Samples Random Value r = 23]
	    C --> E[Prover Calculates Arbitrary Polynomial]
	    C --> F[Verifier Checks Prover's Evaluation]
	    G[Limitation: Integer Coefficients] --> H[Cryptographic Primitives]
	  
	    subgraph Example
	      EX1("Prover's Polynomial: x^3 - 3x^2 + 2x")
	      EX2("Target Polynomial: (x - 1) * (x - 2)")
	      EX3("Arbitrary Polynomial: x")
	    end
	  
	    classDef mainConcept fill:#FFA726,stroke:#333,stroke-width:2px;
	    classDef process fill:#4CAF50,stroke:#333,stroke-width:2px;
	    classDef limitation fill:#F44336,stroke:#333,stroke-width:2px;
	    class A,B mainConcept;
	    class D,E,F process;
	    class G,H limitation;
	  
	  ```
	-
- ((642a0477-a34f-4cb8-96d3-f2e5ce233731))
  collapsed:: true
	- example [[interactive proof protocols]] that don't use polynomials
	  collapsed:: true
		- imagine we have an array of bits of length 10
		- we want to prove to a verifier (e.g. a program) that all those bits  are set to 1
			- i.e., we know an array such that every element equals to 1
		- Hello Verifier can only check/read one element at a time
		- in order to verifiy the [[statement]]
			- one can proceed by reading elements in some arbitrary order and checking if its equal to one
			- if so the confidence in the statement after the first check is 1/10 or the statement is invalidated altogether if the bit equals to 0
			- a verifier must proceed to the next rount until he reaches sufficient confidence
			- in some cases, one may trust a prover and require only 50% confidence which means that 5 checks must be executed, in other cases where 95% confidence is needed all cells must be checked
		- it is clear that the downside of such a proving protocol
			- is
				- that
					- one must do the number of checks proportionate to the number of elements, which is non-practical if we consider arrays of millions of elements
						- [[complexity theory]] would put this as having O(n)
	- ((642a074b-ad20-436a-9c13-3df59e640f1a))
		- the [[polynomial/degree]] is determined by its greatest exponent of x
		- different polynomials  can intersect up to the limit the highest degree of the two polynomials
		- finding the intersection of polynomials creates another polynomial that has an upperbound limit of the highest root of the polynomials and
		- the fact that two non-equal polynomials can only have a limited number of equal evaluations
		  id:: 642a0d3e-2dc0-4f4d-be82-9673213f6674
		  collapsed:: true
			-
			- means that
				- comparing
					- the evaluations of a few points can provide a high degree of confidence that the two polynomaisl are the same or different
		- ((6429fe54-48a2-4491-914b-599b2dc7f62c))
			- verifier generates an element, evaluates it against a polynomial locally
			- verifier gives an element to the prover and asks to evaluated tthe polynomial in question
			- prover evaluates his polynomial at x and gives the result to the verifier
			- verifier checks if the local result is equal to the prover's result, and if so then the statement is proven with a high confidence
- ((642a10f7-dd73-4c72-bd0d-451fdbbf1400))
  collapsed:: true
	- Proving Knowledge of a Polynomial
	  collapsed:: true
		- parties having to trust each other
			- means that
				- there are no measures yet to enforce the rules of the protocol
					- prover does not have to necessarily use the proof medium (polynomial)
					- if the degree of the polynomial is low, then there is a non-negligible probability that it will be accepted
		- to address weaknesses to the protocol, we have to inquire on the nature of knowing a polynomial
		- if one stated that he or she knows a polynomial of degree 1
			- that means that
				- what one really knows is the coefficients c_0, c_1
		- let's say that the prover claims to know a degree 3 polynomial
			- such that
				- x = 1,2 are two possible solutions
			- one such valid polynomials is x^3 - 3x^2 + 2x = 0
	- Factorization
	  collapsed:: true
		- [[The Fundamental Theorem of Algebra]] states that any polynomial can be factored into linear polynomials (i.e. a degree of 1 polynomials representing a line), as long as it is solvable
			- this means we can represent any valid polynomial as a product of its factors
				- (x-a_0)*(x- a_1)...(x-a_n) = 0
					- if any of these factors is zero then the whole equation is zero, henceforth all the a's are the only solutions
				- in fact, our factor can be factored into the following polynomial
					- x^3 - 3x^2 + 2x = (x - 0) * (x-1) * (x- 2)
						- [[polynomial/root]]
		- the example of a prover claiming to know a polynomial of degree 3 with roots 1 and 2
			- this means that his polynomial has the form (x -1) * (x-2)
		- in other words, (x - 1) and (x- 2) are the cofactors of the polynomial in question
			- hence if
				- the prover wants to prove that indeed his polynomial has those roots without disclosing the polynomial itself
				- he needs to prove that his polynomial `p(x)` is the multiplication of those cofactors
					- stated polynomial = target polynomial * arbitrary polynomial
		- in other words, there exists some polynomial that makes the provers stated polynomial equal to target polynomial
			- therefore the prover's stated polynomial contains the target polynomial
				- consequently
					- prover's stated polynomial has all the roots of target polynomial, the very thing to proven
		- a natural way to find the arbitrary polynomial is through [[polynomial/division]]
			- if the prover cannot find such an arbitrary polynomial
				- that means that
					- provers stated polynomial does not have the necessary cofactors (target polynomial) in which case the [[polynomial/division]] will have a remainder
		- using our polynomial identity check protocol
			- we can compare the provers claimed polynomial, the target polynomial, and the arbitrary polynomial
				- [[verifier]] samples a random value `r`, calculates target polynomial at r, and gives r to the prover
				- prover calculates the arbitrary polynomial via dividing their stated polynomial by the target polynomial. they calculate values of r in the stated polynomial and the arbitrary polynomial, the resulting values provided to the verifier
				- verifier checks that the stated polynomial at r is equal to  the target polynomial at r * the arbitrary polynomial at r
		- to put this into practice
			- let us execute this protocol for our example
				- prover claimed polynomial = x^3 - 3x^2 + 2x
				- target polynomial = (x - 1) * (x-2)
			- verifier samples a random value 23, calculates 23 at target polynomial and gives 23 to the prover
			- prover calculates the arbitrary polynomial by dividing  prover's polynomial by the target polynomial and evaluates verifier's given value at its polynomial and the arbitrary polynomial and gives it to the verifier
			- verifier then checks that the provers evaluated value at their polynomial is equal to the value evaluated at the target polynomial and the arbitrary polynomial
			- on the contrary
			  collapsed:: true
				- if the prover uses a different polynomial, which does not have the necessary cofactors, division will have a remainder
					- this means that the prover will have to divide by the remainder by the target polynomial in order to estimate the arbitrary polynomial
					- therefore because of the random selection of x by the vierifer
						- there is a low probability
							- that
								- the evaluation of the remainder will be evenly divisible by the evaluation of t(x)
									- henceforth if verifier will additionally check that handed values by prover must be integers, such proofs will be rejected
										- however, the check requires the polynomial coefficients to be integers too, creating a significant limitation to the protocol
											- that is the reason to introduce cryptographic primitives which make such division impossible, even if the raw evaluations happen to be divisible
		- remark 3.1
		  id:: 642ac4ee-c68b-4cd4-abd4-0dd28fbf9e28
			- now e can check a polynomial for specific properties without learning the polynomial itself, so this already gives us some form of [[zero knowledginess]] and [[succinctness]]
			- never the less, there are multiple issues with this constructure
				- prover may not know the claimed polynomial `p(x)` at all. they can calculate evaluation at the target, select a random number h and set polynomial equals target by h, which will be accepted by the verifier as valid, since the equation holds
				  id:: 642ac549-c8f2-475e-ba6f-a07f4205c4fe
				- because prover knows the random point, he can construct any polynomial that has one shared point at r
				  id:: 642ac589-a504-4eb5-bc07-5538e4fad804
				- in the original statement, prover claims to know a polynomial of a particular degree, in the current protocol there is no enforcement of degree. hence prover can cheat by using a polynomial of a higher degree which also satsifies the cofactors check
			-
	- Obscure Evaluation
		- the first two issues of remark 3.1 are possible because values presented at raw, provers knows r and t(r)
		  collapsed:: true
			- ((642ac549-c8f2-475e-ba6f-a07f4205c4fe))
			- ((642ac589-a504-4eb5-bc07-5538e4fad804))
		- it would be given as a black box so one cannot temper with the protocol, but still able to computer operations on those obscure values
		  collapsed:: true
			- something similar to the hash function,
				- such that when computed
					- it is hard to go back to the original input
		- ((642ac6bc-2449-4fd1-a5cb-4426870a658c))
			- this is exactly what [[Homomorphic Encryption]] is designed for.
			- namely it allows to encrypt a value and be able to apply arithmetic operations on such encryption
			- there are multiple ways to achieve homomorphic properties of encryption, and we will briefly introduce a simple one
			- the general idea is that we choose a base natural number g (say 5) and to encrypt a value we exponentiate g to that power of that value.
				- for example, if we want to encrypt the number three: 5^3 = 125
					- where 125 is the encryption of 3
			- if we want to multiply this encrypted number by 2, we raise it to the exponent of 2
				- 125^2 = 15625 = 5^6
			- we were able to multiply an unknown value by 2 and keep it encrypted. we can aso add two encrypted values through multiplication, for example 3+2
				- 5^3 * 5^2 = 5^5 = 3125
			- similarily, we can subtract encrypted numbers throguh division, for example 5-3
				- 5^5/5^3 = 5^5 * 5^-3 = 25
			- however, since the base 5 is public, it is quite easy to go back to the secret number, dividing encrypted by 5 until the result is 1. the number of steps is the secret number
			  id:: 642ac7f2-5af7-4b47-a4b3-d993613c8e8d
		- ((642ac864-8d08-4b64-a1d7-c556fb3a6a29))
			- this is where [[Modular Arithmetic]] comes into play
			- the idea of modular arithmetic is following:
			  collapsed:: true
				- instead of having an infinite set of numbers
					- we declare that
						- we select only first n natural numbers to work with
							- and if
								- any given integer
									- falls out of
										- this range
								- we "wrap" it around
			- ((642ac8f5-0a88-4008-804a-6d9be5572971))
		- ((642ac938-4a61-46f2-be59-c3211c808aa3))
			- encoding with modular arithmetic means its harder to decode, i.e it redresses: ((642ac7f2-5af7-4b47-a4b3-d993613c8e8d))
			- ((642aca49-1f0e-4075-8f8e-c2de048b7a5f))
			- ((642acae8-f122-4a54-bc31-b29c53c7af69))
			- remark 3.2
			  id:: 642acaf1-c5be-4024-a5e7-6361294c4f5b
				- there are limitations to this homomorphic encryption scheme while we multiply an encrypted value by an unencrpted value, we cannot multiple and divide two encrypted values, as well as cannot exponentiate an encrypted value
				- while unfortunate from the first impression, these properties will turn out to be the cornerstone of [[snarks]]
				- the limitations are addressed in section 3.6.1
			-
		- ((642acbc9-a4db-4fea-a01a-f7eeaf64261e))
			- armed with such tools, we can now evaluate a polynomial with an encrypted random value of x and modify the zero knowledge protocol accordingly
			- let us see how we can evaluate a polynomial `p(x) = x^3 - 3x^2 + 2x`
				- as we have established previously to know a polynomial is to know its coefficients, in this case those are: 1, -3, 2.
				- because homomorphic encryption does not allow to exponentiate an encrypted value, we're given the exponentiated values that are then encrypted and then put into homomorphic operations
					- ((642ace37-a942-41c0-984b-6189184f8bf1))
				- ((642aceef-6ac1-4573-b415-8c4c4954039b))
			-
		- ((642acf21-3316-42c5-b8a4-665455c4502c))
			- ((642acf6c-3049-4093-8ecc-1cbf46c235b9))
				- ((642acfbb-9253-4bf4-a22a-1b9478df8c47))
		- ((642ad080-d269-479a-b790-a4337c9b700d))
			- because verifier can extract knowledge about the unknown polynomial of the proover only from the data sent by the prover
				- let us consider those provided values (the proof): g^p, g^'p', g^h
					- \begin{equation}
					  g^{p} = (g^h)^{t^{s}}
					  \end{equation}
						- (polynomial p(x) has roots of t(x))
					- \begin{equation}
					  (g^{p})^{\alpha} = g^{p'}
					  \end{equation}
						- polynomial of a correct form is used
			- ((642ad1c4-7067-4579-bd05-440a42b01660))
			- to maintain relationships
				- let us examine
					- the verifier's check
						- one of the provers values is on  each side of the equation
							- therefore if we "shift" each of them with the same \delta the equations must remain balanced
					- concretely
						- prover samples a random \delta and exponentiates his proof values with it
							- \begin{equation}
							  (g^{p(s)})^{\delta},
							  (g^{h(s)})^{\delta},
							  (g^{\alpha*p(s)})^{\delta}
							  \end{equation}
				- after consolidation we can observe that the check still holds
					- \begin{equation}
					  (g^{p(s) \delta }) = (g^{t(s)  h})
					  ,
					  (g^{p * \delta * \alpha }) = g^{\delta * p'}
					  \end{equation}
			- note: how easily the [[zero knowledginess]] is woven into the construction, this is often refered to as "free" zero-knowledge
		- ((642b1c7e-86c7-465d-ac41-bbdf8432a78c))
			- till this point
			  collapsed:: true
				- we had an interactive zero-knowledge scheme
					- why is that the case?
						- because the proof is only valid for the original verifier,
							- nobody else (other verifiers) can trust the same proof since
								- as mentioned in ((642ac4ee-c68b-4cd4-abd4-0dd28fbf9e28)) the verifier
								  collapsed:: true
									- could collude with
										- the prover
									- disclose
										- these secret parameters s, \alpha
											- which allows to fake
												- the proof
								- the verifier can generate [[false proof]] for the same reason
								- verifier have to store \alpha and t(s) until all relevant proofs are verified, which allows an extra attack surface with possible leakage of secret parameters
							- therefore
								- a separate interaction with every verifier
									- is required in order for
										- a [[statement]] (knowledge of polynomial in this case)
											- to be proven
				- while [[interactive proof protocols]] has its use cases
					- for example when
						- a prover wants to convince a dedicated verifier (called designated veriifier)
							- such that
								- the proof cannot be re-used to prove the same statement to others
						- it is quite inefficient when
							- one needs to convince many parties simultaneously
								- (e.g. in distributed systems such as [[blockchain]] )
							- or permanently
					- prover would be required to stay online at all times and perform the same computation for every verifier
						- hence we need the secret parameters to be reusable, public, trustworthy and infeasible to abuse
			- let us first consider how we would secure the secrets (t(s), \alpha) after they are produced
			- we can encrypt them the same way verifier encrypts powers of s before sending to the prover
			- however as mentioned in ((642acaf1-c5be-4024-a5e7-6361294c4f5b)), the [[Homomorphic Encryption]] we use does not support the multplication of two encrypted values, which is necessary for both verification checks to multiple encryptions of t(s) and h as wel as p and \alpha
				- this is where [[cryptographic pairings]] fit in
			- ((642b31d0-c268-483a-ba4f-46556ba8b6fa))
			  id:: 642d952c-cab5-40b8-9b65-f9539065348e
			  collapsed:: true
				- [[cryptographic pairings]] (bilnear map) is a mathematical construction
					- denoted as
						- a function e(g*, g*) which
							- given
								- two encrypted inputs g^{a}, g^{b} from one set of numbers
							- allows to map them deterministically to their multiplied representation in a different output set of numbers
								- i.e. e(g^{a}, g^{b}) = e(g, g)^{ab}
								- ![image.png](../assets/image_1680552720145_0.png)
				- because the source and output number sets are different,  the results of the pairings is not usable as an input for another pairing operation.
				- we can look at the output set (also called target set) as being from a different universe
				- therefore
					- we cannot multiply the result by another encrypted value and suggested by the name itself we can only multiply two encrypted values at a time
				- it some sense, it resembles a [[hash function]], which maps all possible input values to an element in the set of possible output values and it is not trivially reversible
				- ((642b34b1-a764-4234-b583-0348b9487b5c))
				- a rudimentary (and technically incorrect) mathematical analogy for pairing function
					- would be to state that
						- there is a way to "swap" each input's base and exponent
							- such that
								- base g
									- is modified in
										- the process of transformation into exponent
											- e.g. g^{a} -> a^{g}
					- both "swapped" inputs are then multiplied together,
						- such that
							- raw a and b values
								- get multiplied under
									- the same exponent, e.g.:
										- e(g^{a}, g^{b}) = a^{g} * b^{g} = (ab)^{g}
					- therefore
						- because
							- the base gets altered during the swap
						- using the result (ab)^{g} in another pairing
							- (e.g. e((ab)^{g}, g^{c}))
							- would not produce
								- desired encrypted multiplication abc
					- the core properties of pairings can be expressed in the equations
						- e(g^{a}, g^{b}) = e(g^{b}, g^{a}) = e(g^{ab}, g^{1}) = e(g^{1}, g^{ab}) = e(g^{1}, g^{a})^{b} = e(g^{1}, g^{1})^{ab}
				- technically the result of a pairing is an encrypted product of raw values under a different [[generator]] g of the target set
					- i.e. e(g^{a}, g^{b})  = g^{ab}
				- therefore it has properties of [[Homomorphic Encryption]]
					- e.g. we can add engrypted products of multiple parings together
						- e(g^{a}, g^{b}) * e(g^{e}, g^{d}) = g^{ab} * g^{cd} = g^{ab + cd} = e(g, g)^{ab + cd}
				- note: [[cryptographic pairings]] is leveraging [[Elliptic Curves]] to achieve these properties
					- therefore from now on notation g^{n}
						- will represent
							- a [[generator element]] on a curve added to itself n times
						- instead of
							- a multiplicative group generator
								- which we have used in
									- previous sections
					-
			-
			-
			- ((642b3afb-0790-4053-87dd-760b35541fed))
			  collapsed:: true
				- having [[cryptographic pairings]]
					- we are now ready to set up secure public and reusable parameters
				- let us assume
					- that
						- we
							- trust
								- a single honest party
							- to generate
								- secrets s and \alpha
				- as soon as
					- \alpha and all necessary power of s with with corresponding \alpha-shifts
						- are encrypted
							- g^\alpha, g^{s^{i}}, g^\alpha s^{i}}
								- for i in 0, 1, ...d
									- the raw values must be deleted
				- these parameters are usually referred to as [[Common Reference String]] or CRS
				- after CRS is generated any prover and any verifier can use it in order to conduct [[non-interactive]] zero-knowledge proof protocol
				- while non-crucial, the optimized version of CRS will included evaluation of the target polynomial g^{t(s)}
				- moreover CRS is divided into two groups (for i in 0, 1, ..., d):
					- [[proving key]]
					- [[verifying key]]
				- being able to multiple encrypted values, the verifier can check the polynomials in the last step of the protocl
					- having [[verifying key]] verifier processes received encrypted polynomial evaluations g^{p}, g^{h}, g^{p'} from the prover
						- checks that p = t by h in encrypted space
							- e(g^{p}, g^{1}) = e(g^{t}, g^{h})
								- which is equivalent to
									- e(g, g)^{p} = e(g, g)^{t h}
						- checks [[polynomial/restriction]]
							- $$ e(g^{p}, g^{\alpha}) = e(g^{p'}, g) $$
					-
			- ((642b43de-393c-431a-b48a-07a60de0af02))
			  collapsed:: true
				- while [[trusted setup]] is efficient
					- it is not effective since
						- multiple users of [[Common Reference String]]
							- will have to
								- [[trust]]
									- that
										- one deleted \alpha and s
											- since there is no way to prove that
				- hence it is necessary to minimize or eliminate that [[trust]]
				- otherwise a dishonest party would be able to produce [[false proof]] without being detected
				- one way to achieve that is by generating a composite CRS by multiple parties employing mathematical tools introduced in previous sections
					- such that
						- neither of those parties knows the secret
				- ((642b454a-2ffb-4252-a4b9-7e6c4b2b7424))
				-
		- ((642b45a2-3dc6-45a9-8f16-21c15d5399bd))
			- we are now ready to consolidate the evolved [[snarks]] protocol
			- being formal, for brevity, we will use curly brackets to denote a set of elements populated by the subscript next to it, for example $$  \{ s^{i}  \}_{i \ni |d|}  $$
			- having agreed upon the target polynomial t(x) and degree d of prover's polynomial
				- setup
					- sample random values s, \alpha
					- calculate encryptions g to the \alpha and $$  \{ g^{s^{i}}  \}_{ i \ni |d|} , \{ g^{ \alpha s^{i}}  \}_{i \ni  \{0,..., d\}}   $$
					- [[proving key]] : $$  \Biggl( \{ g^{s^{i}}  \}_{ i \ni |d|} , \{ g^{ \alpha s^{i}}  \}_{i \ni  \{0,..., d\}}  \Biggl)  $$
					- [[verifying key]]: $$ \left( g^{ \alpha },  g^{t(s)} \right) $$
				- proving
					- assigning coefficients
						- $$ \{ c_i  \}_{i \ni  \{0,..., d\}}  $$ (i.e. knowledge), $$ p(x) = c_d x^{d} + \cdots + c_1x^{1} + c_0x^0 $$
					- calculate polynomial $$ h(x) = \frac{p(x)}{t(x)} $$
					- evaluate encrypted polynomials g^{p(s)} and g^{h(s)} using $$ \{ g^{s^{i}}  \}_{ i \ni |d|}  $$
					- evaluate encrypted shifted polynomial $$ g^{ \alpha p(s)} $$  using $$ \{ g^{ \alpha s^{i}}  \}_{i \ni  \{0,..., d\}} $$
					- sample random \delta
					- set the randomized proof $$ \pi = ( (g^{\delta p(s)}), (g^{\delta h(s)})(g^{ \delta \alpha*p(s)})) $$
				- verification
					- parse proof \pi as $$ ( g^{p}, g^{h}, g^{p'} ) $$
					- check polynomial restriction  $$ e(g^{p'}, g ) = e(g^{p}, g^{ \alpha }  ) $$
					- checking polynomial cofactors $$ e(g^{p}, g ) = e(g^{t(s)}, g^{ h }  ) $$
			- remark 3.3
			  collapsed:: true
				- if it would be possible to reuse result of pairing for another multiplication
					- such protocol would be completely insecure
						- because
							- the prover can assign $$ g^{p'} = e(g^p, g^{ \alpha } ) $$
								- which would then pass
									- the polynomial restriction check
										- $$ e(e(g^p, g^{ \alpha }), g) = e( g^{p}, g^{ \alpha }) $$
			- ((642b529c-e67b-4878-bac4-eb26f4d218c2))
				- we came to zero-knowledge succinct non-interactive arguments of knowledge protocol for the knowledge of a polynomial problem
					- which is a niche use-case
				- while one can claim that a prover can easily construct such a polynomial p(x) just multiplying t(x) by another bounded polynomial to make it pass the test, the construction is still useful
				- verifier knows that the prover has a valid polynomial but not which particular one
				- we could add additional proofs of other properties of the polynomial
					- divides by multiple polynomials
					- is a square of a polynomial
				- there could be a service which accepts, stores, and rewards all the attested polynomials, or there is a need in an encrypted evaluation of unkown polynomials iof a necessary form.
				- however, having universal scheme would allow for a myriad of applications
			-
			-
		-
- ((642b5365-ac49-4056-a7eb-fab62bf1cb73))
	- we have paved our way with a simple yet sufficient example involving most of the zk snark machinery, and it is now possible to advance the scheme to execute zero knowledge programs
	- ((642c68fe-77e4-4796-8506-9fefba571f7f))
	  collapsed:: true
		-
		- let us consider a single program in pseudocode
		- algorithm 1
			- ```
			  function calc(w, a b)
			  	if w then
			      	return a x b
			       else
			       	return a + b
			       end if
			  end function
			  ```
		- from a high level view, it is quite unrelated to polynomials, which we have the protocol for
			- therefore we need to find a way to convert a program into the polynomial form
				- the first step is then to translate the program into the language of math, which is relatively easy, the same statement can be expressed as following (assuming w is either 0 or 1)
					- $$ f(w,a,b) = w(a \times b) + (1 - w)(a + b) $$
					- executing calc(1, 4, 2) and evaluting f(1,4,2) will yield the same result: 8
		- what we need to prove then (in this example), is that for the input (1, 4, 2) of expression f(w, a, b) the output is 8, in other words, we check the equality
			- $$ 8 = w(a \times b) + (1 - w)(a + b) $$
	- ((642c8155-3b43-4622-94ba-09df27f98dd0))
	  collapsed:: true
		- we now have a general computation expressed in mathematical language, but we still need to translate it into the real of polynomials
		- let's have a closer look at what [[computation]] is in a nutshell
			- any computation at its core
			  id:: 642c8187-35e7-4039-97c8-d631c483a1d1
				- consists of
					- elemental operations of the form
					  id:: 642c818e-883a-4a1c-aa1f-eb229f7de702
						- operator
							- left operand
							- right operand
						- output
		- two operands (values) are being operated upon an operator (e.g +, -, x, division)
		- because any complex computation (or program) is just
			- a series of operatoins
				- firstly we need to find out how single such operation
					- can be represented by
						- a polynomial
		- ((642c82b5-1939-484a-b498-d6123a4c37dc))
			- let's see how a [[polynomial]] is related to  a [[arithmetic operation]]
				- if you take two polynomials f(x) and g(x) and try, for example
					- to multiply them
						- $$ h(x) = f(x) \times g(x) $$
						- the result of evaluation of h(x) at any x = r
							- will be
								- the multiplication of f(r) and g(r)
						- let us consider two polynomials
							- $$ f(x) = 2x^2 - 9x + 10, g(x) = -4x^{2} + 15x -9 $$
							- for x = 1 $$ f(1) = 2 - 9 + 10 = 3, g(1) = -4 +15 -9 = 2$$
	- ((642c869b-e36f-42e3-b253-49a55f49aa1e))
	  collapsed:: true
		- if the prover claims to have the result of multiplication of two numbers
			- how does the verifier check that?
		- to prove
			- the correctness of a single operation
			- we must enforce
				- the correctness of the output (result) for the operands provided
		- if we look again at the form of operation
			- {{embed ((642c818e-883a-4a1c-aa1f-eb229f7de702))}}
			- the same can be represented as an operation polynomial
				- operator
					- l(x)
					- (rx)
				- o(x)
				- where for some chosen a
					- l(x) represents the value of the left operand
					- r(x) represents the value of the right operand
			- therefore if
				- the operands and the output
					- are represented correctly for
						- the operation by those polynomials
				- then
					- the evaluation `l(a)` operator `r(a) -o(a) = 0` is surfacing the fact that
						- the operation polynomial l(x) operator r(x) - o(x) = 0
							- ihas to evaluate to zeri at a
						- if
							- the value represented by the output polynomial o(x) is the correct result produced by the operator on the values represented by operand polynomials l(x) and r(x)
				- henceforth operation polynomial must have the root a if it is valud, and consequently, it must contain cofactor (x-a) as we have established previously, which is the target polynomial we prove against, i.e. t(x) = x - a
	- ((642ce2bf-3c1a-4200-977a-a48f1bd85617))
	  collapsed:: true
		- let us modify our latest protocol to support a single multiplication operation proof
		- recal that previusly
			- we had proof of knowledge of polynomial `p(x)`
				- but now we deal with three l(x), r(x), o(x)
		- while we could define p(x) = l(x) x r(x) - o(x)
			- ((643189ba-a2ff-4f59-a26b-7c4f797f2878))
				- there are two counterargument
				  collapsed:: true
					- firstly, in our protocol, the multiplication of encrypted value is not possible in the proving stage
						- since pairings can only be used once and it is required for [[polynomial/restriction]] check
					- secondly, this would leave an opportunity for the prover to modify the structure of the polynomial at will but still maintain a valid [[cofactor]]
						- for example
							- p(x) = l(x)
							- p(x) = l(x) - r(x)
							- p(x)  = l(x) * r(x) + o(x)
							- as long as p(x) has root a
						- such a modification effectively means that the proof is about a different statement
				- this means that
					- the knowledge of polynomial must be adjusted
				- in essence what a verifier needs to check in encrypted space is that
					- $$ l(x) \times r(s) - o(s) = t(s)hs(s) $$
				- while a verifier can perform multiplication using cryptographic pairiings,  the subtraction -o(x) is an expensive operation
					- that is why we move o(x) to the right side of the equation
						- $$ l(x) \times r(s) = t(s)hs(s) + o(s) $$
				- in encrypted space the verifiers check translates to:
					- $$ e(g^{l(s)}, g^{r(s)}) = e(g^{t(a)}, g^{h(s)}) * e (g^{o(a), g}$$
					- note: recall that the result of cryptographic pairing supports encrypted addition through multiplication, see ((642d952c-cab5-40b8-9b65-f9539065348e))
				- ((64318ebe-4364-4b1c-9b62-b0e49da191f7))
					- ((64318f0c-c606-4040-a900-773401f6e9de))
					- proving
						- assign corresponding coefficients to the l(x), r(x), o(x)
						- calculate polynomial $$ h(x) = \frac{l(x) \times r(x) - o(x)}{t(x)} $$
						- evaluate encrypted polynomials
						- evaluate encrypted shifted polynomials
						- set proof \pi
					- verification
						- parse proof \ou
						- [[polynomial/restriction]] check
						- valid operation check
				-
	- ((64318fb4-a6cf-4c56-b712-33b8ca5f4504))
	  collapsed:: true
		-
		- previous section proves a single operations, how do we get to proving multiple operations?
		- let us try to add just one other operation
		- consider the need to compute the product $$ a \times b \times c $$
			- in the elemental operation model this would mean two operations
				- $$ a \times b = r_1 $$
				- $$ r_1 x c = r_ 2 $$
		- as discussed previosuly
			- we can represent
				- one such operation
					- by making
						- operand polynomials
							- evaluate to
								- a corresponding value at some arbitrary x, for example 1 and another value at a different x, for example 2
			- such independence allows us to execute two operations at once without mixing together
				- where it is visible that the operation polynomial has rootts x = 1 and x = 2
					- therefore both operations are executed correctly
		- let us have a look at example of 3 multiplcations
			- $$ 2 \times 1  \times 3  \times 2 $$
			- $$ 2 \times 1 = 2 $$
			- $$ 2 \times 3 = 6 $$
			- $$ 6 \times 2 = 12 $$
		- we need to represent those as operand polynomials
			- such that
				- for operations represented by
					- $$ x \in {1,2,3} $$ the l(x)
						- pass correspondingly through
							- 2, 2, and 6
							- i.e.
								- through points (1,2), (2,2), (3,6)
							- and similarly
								- $$ r(x) \in { (1,1), (2,3), (3,2) } , o(x) \in { (1,2}, (2,6), (3,12) ) $$
		- however, how do we find such polynomials which passes through those points?
		- for any case where we have more than one point, a particular mathematical method has to be used
		- ((64319869-ee28-4988-bb95-13113ef0d4b1))
		  collapsed:: true
			- in order construct 
			  id:: 6431987a-7b48-4ceb-bced-74050be58c6d
				- operand polynomials
				- output polynomials
			- we need a method which
				- given a set of points
			- produces
				- a curved polynomial
					- in such a way that
						- it passes through all these points
			- it is called [[polynomial/interpolation]]
				- there are different ways avaialble
					- set of equations with unknowns
					- newton polynomial
					- neville's algorithm
					- [[Lagrange Interpolation]]
					- [[Fast Fourier Transformation]]
			- let's use the former for example
				- the idea of such method
		- ((64319b01-dc74-430a-89ba-f10b07bc748f))
			- now we have operand polynomials which represent three oeprations
			- let us see step by step how the correctness of each operation is verified
			- recall that
				- a verifier is looking for equality echeck
					- $$ l(x) \times r(x) - o(x) = t(x)h(x) $$
					- in this case, because operations are represented at points
						- $$ x \in {1, 2,3} $$
						- the target polynomial has to evaluate to 0 at those x-s
							- in other words, the roots of x must be 1 2 3
			- first l(x) and r(x) are multiplied
			- secondly o(x) is substracted from the result l
				- where it is already visible that every operands multiiplication corresponds to a correct result
			- for the last step a prover needs to present a valid cofactor h(x)
		-
	- ((64319e80-8312-4ca0-88ea-67313c65be90))
	  collapsed:: true
		- with such an approach
			- we can prove many operations at once, but there is a critical downside to it
		- if the program, execution for which is being proved, uses the same variable, either as an operand or as its output, in different operations
			- a by b = r1 ; a by c = r2
		- the a will have to be represented in the left operand polynomial for both operations as
		- nevertheless, because our protocol allows prover to set any coefficient to a polynomial, he is not restricted from setting different values of a for different operations
		- this freedom breaks consistency and allows prover to prove the execution of some other program which is not what verifier is interested in
		- therefore we must ensure that any variable can only have a single value across every operation it is used in
			- note: variable in this context differens from the regular computer science definition in a sense that is immutable and is only assigned once per execution
		- ((6431ad10-31b4-4bad-8e20-7a95f5d3d1d6))
		  collapsed:: true
			- let us consider a simple case
				- where
					- we have only one variable (e.g. a)
						- used in all left operands represented by the left operand polynomial
					- we have to find out if it is possible to ensure that this polynomial represents the same values of a for every operation
					- the reason why a prover can set different values is that he has control over each coefficent for every exponentiation x
						- therefore if those coefficient were constant, that would solve the variability problem
			- when we want to change all the values simultaneously in a polynomial, we need to change its proportion
			- so how coefficent proportion can be preserved?
				- we can start by considering what is provided as proof as the left operand polynomial
				- it is an encrypted evalulation of l(x) at some secret s: $$ g^{l(s)} $$, it is an encrypted number
				- we already know from ((642acf21-3316-42c5-b8a4-665455c4502c)) how to restrict a verifier to use only the provided exponents of s through an alpha shift, such that homomorphic multiplication is the single operation available
			- similarly to retricting a single exponent
				- the verifier can restrict the whole polynomial at once
				- instead of provoding separate encryptions and their alpha shits
			- the protocol proceedes
				- setup
					- construct the respective operand polynomial l(x) with corresponding coefficents
					- sample random \alpha and s
					- set [[proving key]] with encrypted operand polynomial and its shifted par
					- set the [[verifying key]]
				- proving
					- having operands value v
						- multiply operand polynomial
						- multiply shifted operand polynomial
					- provide operand polynomial multiplication proof
				- verification
					- parse proof as encrypted operand polynomial and encrypted operand polynomial
					- verify proportion
			- prover needs to respond w/ same alpha-shift and because he cannot recover \alpha from the [[proving key]]
				- the only way to maintain the shift is to multiply both encryptions of the operand polynomial and shifted polynomial by the same value
			- therefore prover cannot modify individual coefficents of operand polynomial
				- prover cannot add or subtract because it implies access to unencrypted \alpha
			- ((6431b0a4-83aa-46f3-a84f-af8c2c5eafb5))
				- since verification key contains g to the alpha it is possible to add or subtract an arbitrary value v' to the polynomial
				- therefore it is possible to modify the polynomial beyond what is intended by the verifier and prove a different statement we will address this shortcoming in section ((6431b10a-7350-49d3-a6a3-7f84eee97093))
				-
		- ((6431b139-668f-4a29-9227-8d48f37d9e01))
	- ((6431b1d8-45ec-4bc9-b80a-d806b7463ce7))
	  collapsed:: true
		- there are multiple additional useful properties which are acquired as a side-effect of such modifications
		- ((64321302-7e90-4334-a4b3-45799d73751f))
			- in the above construction,
				- we have been using evaluatoins of unassigned variabe polynomials 1 or 0
					- as a means to signify if
						- the variable is used in operation or not
			- naturaly, there is nothing that stops us from using other coefficients as well, including negative ones, because we can interpolate polynomials through any necessary points
				- examples of such operations re
					- 2z x 1b = 3r
					- -3a x 1b = -2r
			- therefore, our program can now use constant coefficients
			- note: constant coefficeint for the same variable can be different in different operations and operands/outputs
		- ((6432148b-0411-4a17-9adc-db4443006d89))
		  collapsed:: true
			- considering the updated construction, it is apparant that in
				- polynomial representation
					- every operand expressed by some distinct x is a sum of all operand variable polynomials
						- such that
							- only single used variable can have a non-zero value and all other are zero
			- we can take advantage of such construction and allow to add any number of necessary variables each each operand in operation
			- forexample in the first operation, we can add a + c first and only then multiply it by some other operand e.g., (a + c) x b = r
				- therefore it possible to add any number of present variables in a single operand, using arbitrary coefficients for each of them, to produce an operand value which will be used in a corresponding operation, as needed in a respective program
			- note: each operation's operand has its own set of coefficnets c
		- ((64321628-9d15-4d7a-ba0d-173e1e6d5807))
	- ((64321679-8384-4763-8386-91bdbc0d613a))
	- ((6432173b-b052-415c-bf85-02eb04b6cf97))
		- ((6432178d-b5d7-46bd-82da-debf23b52f32))
		- ((64324a9f-ea78-4f71-8d79-788641fc4d00))
		- ((6431b10a-7350-49d3-a6a3-7f84eee97093))
		- ((64324c49-651a-436a-b1f6-f9056ae8b3e5))
	- ((64324c64-9ab9-4338-841e-b68e387724f0))
	- ((64324cfc-04df-4cb7-b467-dbc7aaf4e6df))
	- ((64324d2b-173c-46b3-9d21-5645d38e5375))
	- ((64324d8a-ea53-4b3c-a23c-7440c80ef691))
	-