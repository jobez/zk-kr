- https://perama-v.github.io/cairo/background/integer_lift/
- example usage 
  id:: 636bdd9e-2536-4ab7-b4e3-5ac336459722
  ```cairo
  %builtins output range_check 
  from starkware.cairo.common.uint256 import (uint256_add, Uint256, uint256_mul) 
  from starkware.cairo.common.serialize import serialize_word 
  
  func main{output_ptr : felt*, range_check_ptr}(){ 
  	alloc_locals; 
  	local num1 : Uint256 = Uint256(low=0,high=10); 
      local num2 : Uint256= Uint256(low=0,high=3); 
      let (local mul_low : Uint256, local mul_high : Uint256) = uint256_mul(num1, num2); 
      serialize_word(mul_high.low) ; 
      return ();
      }
  ```