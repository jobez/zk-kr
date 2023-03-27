- is a domain specific language for creating and verifying proofs
- achieves simplicity via
  collapsed:: true
	- does not compile immediately to a fixed NP-complete language
	- instead it compiles to
		- an intermediate language which itself can be compiled to an arithmetic circuit or a rank-1 constraint syste,
- noir can be used for a variety of purposes
  collapsed:: true
	- eth developers
		- includes a command to publish a contract which verifiers your Noir program
		- this will be modularised in the future, however as of the alpha you can use the contract command to create it
	- protocol developers
		- As a protocol developer, you may not want to use the Aztec backend due to it not being a fit for your stack or maybe you simply want to use a different [[proving system]]
		- Since Noir does not compile to a specific proof system, it is possible for protocol developers to replace the PLONK based proving system with a different proving system altogether.
	- blockchain developers
		- As a blockchain developer, you will be constrained by parameters set by your blockchain, ie the proving system and smart contract language has been pre-defined.
		- In order for you to use Noir in your blockchain, a proving system backend must be implemented for it and a smart contract interface must be implemented for it.
- Features
  collapsed:: true
	- backends
		- [[barretenberg]]
		- [[marlin]]
	- compiler
		- module system
		- for expressions
		- array
		- bit operations
		- binary operations
		- unsigned integers
		- if statements
		- structures and tuples
		- generics
	- ACIR supported opcodes
		- sha256
		- blake2s
		- schnorr signature verification
		- merkle membership
		- pedersen
		- hashtofield
- building the constraint system
	- `nargo check`
	- add inputs
		- in the created prover.toml add values for the inputs to the main function
	- create proof
		- `nargo prove p`
	- verify the proof
		- `nargo verify p`
- language
	- private and public types
		- ```
		  fn main(x : Field, y : pub Field) -> pub Field { x + y}
		  
		  ```
	- primtive types
		- field
		- integer
		- boolean
	- compound types
	  collapsed:: true
		- struct
			- defining methods within structs
				- ```
				  struct MyStruct {
				  	foo: Field,
				      bar: Field
				  }
				  
				  impl MyStruct {
				  	fn new(foo: Field) -> MyStruct {
				      	MyStruct {
				          	foo,
				              bar: 2,
				          }
				      
				      fn sum(self) -> Field {
				      	self.foo + self.bar
				      }
				      }
				  
				  }
				  
				  fn main() {
				  	let s = MyStruct::new(40)
				      constrian s.sum() == 42;
				      constrain MyStruct::sum(s) == 42
				  
				  }
				  ```
			- deconstructing strucs
				- ```
				  fn main() {
				  	let Animal { hands, legs: feet, eyes } = get_octopus(); 
				  	let ten = hands + feet + eyes as u8;
				  }
				  
				  fn get_octopus() -> Animal {
				  	let octopus = Animal {
				      	hands: 0,
				          legs: 8,
				          eyes: 2
				      };
				  	octopus
				  }
				  ```
			- ```
			  struct Animal { hands: Field, legs: Field, eyes: u8,} fn main() { let legs = 4; let dog = Animal { eyes: 2, hands: 0, legs,}; let zero = dog.hands;}
			  ```
		- tuple
			- ```
			  fn main() { let tup: (u8, u64, Field) = (255, 500, 1000);}
			  ```
		- array
			- ```
			  fn main(x : Field, y : Field) { let my_arr = [x, y]; let your_arr: [Field; 2] = [x, y];}
			  
			  ```
	- functions
		- fn keyword
	- loops
		- only for loops are possible
			- ```
			  for i in 0..10 {
			  // do something
			  };
			  
			  ```
	- if expressions
		- ```
		  let a  = 0;
		  let mut x: u32 = 0;
		  
		  if a == 0 {
		  	if a != 0 {
		      	x = 6;
		      } else {
		      	x = 2;
		      }
		  
		  } else {
		  	x = 5;
		      constrain x == 5;
		  
		  }
		  constrain x == 2;
		  
		  ```
	- constrain statement
		- `constrain` which will explicitly constrain the predicate/comparison expression that follows to be true
		- ```
		  fn main(x : Field, y : Field) {
		  	constrain x == y;
		  }
		  
		  ```
	- acir
		- noir compiles to [[ACIR]] abstract circuit intermediate rpresentation
			- which later can compile to
				- any ZK proving system
		- the purpose of ACIR is to act as an intermediate layer between the proof system that Noir chooses to compile to and the Noir syntax
		- this separtion between [[proof system]] and programming language
			- allows those who want to integrate [[proof system]] to have  a stable target
- compiling a proof
	- `cargo compile my_proof` will perform two processes in a given Noir project
		- first compile the Noir program to its ACIR and solve the circuit's [[witness]]
		- second, create a new build/ directory to store the ACIR `my_proof.acir` and the solved witness `my_proof.r`
		- these can be used by the Noir typescript wrapper to generate a prover and verifier inside of typescript rather than in nargo
- ui
	- https://github.com/noir-lang/noir-web-starter-next
- solidity [[verifier]]
	- you can create a verifier contracct for your noir program by running
	- a new contract folder would then be generated in your project directory, containing the solidity file `plonk_vk.sol`
	- it can be deployed on any evm blockchain acting as a verifier smart contract
	- note: it is possible to compile verifier contracts of Noir programs for other smart contract plaforms as long as the proving backend suppies an implementation
	- [[barretenberg]] the default proving backend nargo is integrated with supports compilation of verifier contraccts in solidity for the time being
- roadmap
	- concretely the following items are on the road map
		- prover and verifier key logic
			- prover and verifier pre-proces per compile
	- fallback mechanism for backend unsupported opcodes
	- visbility modifiers
	- signed integers
	- backend integration [[bulletproofs]]
	- recursion
	- big integers
- https://github.com/noir-lang/awesome-noir