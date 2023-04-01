- allows programmers to define the [[constraints]] that define the [[circuit]]
- all constraints must be of the form `A*B + C = 0`
	- where A, B, c are [[linear combinations of [[signals]] ]]
- you can define [[constraints]] in this way
	- ```
	  pragma circom 2.0.0;
	  
	  template Multiplier2 () {
	  	// declaration of signals
	      signal input a;
	      signal input b;
	      signal output c;
	      
	      // constraints.
	      c <== a * b;
	  
	  }
	  ```
- the programmer can distinguish between public and private signals only when defining the main component, by providing the list of public input signals
	- ```
	  pragma circom 2.0.0;
	  
	  template Multiplier2 () {
	  	// declaration of signals
	      signal input a;
	      signal input b;
	      signal output c;
	      
	      // constraints.
	      c <== a * b;
	  
	  }
	  
	  component main {public [in1,in2]} = Multiplier2();
	  ```
- https://zkrepl.dev/
- ![Flux-Diagram.drawio.png](https://github.com/lambdaclass/circom_export_to_cairo/blob/main/Flux-Diagram.drawio.png?raw=true){:height 231, :width 776}