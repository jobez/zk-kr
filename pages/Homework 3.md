- Do Hello Playground example https://www.cairo-lang.org/playground/
	- run the program and see the output in the console
	- debug the program, look at the memory and the watch window, do you understand how the [[output_ptr]] is being used?
		- the [[output_ptr]] is passed in as an implicit argument that allows us to access predefined operations called [[builtins]]
		- each time `serialized_word` is called, the output_ptr is incremented, as specified here: https://github.com/starkware-libs/cairo-lang/blob/54d7e92a703b3b5a1e07e9389608178129946efc/src/starkware/cairo/common/serialize.cairo#L2
		- what is ultimately done with the output_ptr?
		- i believe it has something to do with the output_runner as assigned as a built in to the `CairoFunctionRunner` https://github.com/starkware-libs/cairo-lang/blob/167b28bcd940fd25ea3816204fa882a0b0a49603/src/starkware/cairo/common/cairo_function_runner.py#L39
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
-