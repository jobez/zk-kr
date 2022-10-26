- Do Hello Playground example https://www.cairo-lang.org/playground/
	- run the program and see the output in the console
	- debug the program, look at the memory and the watch window, do you understand how the [[output_ptr]] is being used?
		- the [[output_ptr]] is passed in as an implicit argument that allows us to access predefined operations called [[builtins]]
- add a function called `square` that will be called from the main function
  the square function should have one parameter `x` and return the square of `x`
- in the `main` function call `square` function  and output the result
- ``` 
  // Use the output builtin.
  %builtins output
  
  // Import the serialize_word() function.
  from starkware.cairo.common.serialize import serialize_word
  
  func square{}(x: felt) -> (squared_x : felt) {
      return (squared_x=x*x);
  }
  
  func main{output_ptr: felt*}() {
      let w : felt = square(x=2);    
      tempvar x = 10;
      tempvar y = x + x;
      tempvar z = y * y + x;
      serialize_word(w);
      serialize_word(x);
      serialize_word(y);
      serialize_word(z);
      return ();
  }
  
  ```