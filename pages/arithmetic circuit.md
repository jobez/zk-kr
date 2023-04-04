- mathematical models used for [[computation]]
  collapsed:: true
	- where inputs and outputs
		- are related through
			- a series of arithmetic operations
				- addition, subtraction, multiplication
		- with
			- [[finite field]] elements
- ((6429dc5d-4d28-4b0f-9fa8-f848ea9d6dae))
	- ```mermaid
	  graph TD
	  A[Zero-Knowledge Proof System] --> B[Arithmetic Circuit Representation]
	  B --> C[Selectors]
	  B --> D[Gate Constraints]
	  B --> E[Copy Constraints]
	  C --> F[Activate/Deactivate Constraints]
	  D --> G[Relation Between Input & Output Wires]
	  E --> H[Consistency of Shared Wires]
	  F --> I[Flexible Representation]
	  G --> I
	  H --> I
	  I --> J[Proof Protocol]
	  J --> K[Secure Proof]
	  
	  ```
	- ```mermaid
	  graph TD
	  A[Zero-Knowledge Proof] --> B[Special Structure]
	  B --> C[Switches Selectors]
	  B --> D[Input-Output Relationships Gate Constraints]
	  B --> E[Consistency Copy Constraints]
	  C --> F[Control Rules]
	  D --> G[Define Process]
	  E --> H[Shared Information]
	  F --> I[Simplified Representation]
	  G --> I
	  H --> I
	  I --> J[Proving Correctness]
	  J --> K[Privacy & Security]
	  
	  ```
	-
	- once we have a potentially large circuit
		- we want to get it into a more usable form
		- so
			- we
				- can put
					- the values
						- into
							- a table
	- so for gates 1 to i we can represent a, b, c as
		- a_i, b_i, c_i
	- and if the circuit is correct then for an addition gate
		- a_i + b_i = c_i
		- a_i + b_i - c_i = 0
	- we would end up with a table like
- ((6429e1ec-1afe-43a3-aa44-da103aa22dc7))
- ((6429e0b9-9526-4f0c-adf8-fa4f551e29e2))