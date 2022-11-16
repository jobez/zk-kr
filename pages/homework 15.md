- imagine you get the following trace
  0,3,6,9,12,15
  from your program (which simply adds 3 to the previous value)
  
  write out the constraints for the trace, in terms of i, j where i represents the step and j the column
	- | step      | amount |  total     |
	  | :---        |    :----:   |          ---: |
	  | 0      | 0       | 0   |
	  | 1   | 3       | 3     |
	  | 2 | 6 | 9|
	  | 3 | 9 |  18 |
	  | 4 | 12 | 30 |
	  |  5  |  15  | 45 |
	- constraints
		- A[0] = 0
		- for i as from 1 to 5
			- A[i] - A[i-1] - A[1] = 0
- polynomial practice:
  for p(x) = x^3 + 5x^2 - 2x -4
	- a) find an integer root, i.e. p(a) = 0 (clue < 5)
		- a = 1 (if we assume there was a typo in the polynomial, otherwise there are no integer roots for the polynomial given)
	- b) write this in terms of a lower degree polynomial q(x)
	  such as p(x)
		- (x-1)(x^2 + 6x + 4) = x^3 + 5x^2 - 2x -4
	- what are the degrees of p(z) and q(x)
		- p(z) is a degree of one
		- q(x) is a degree of two
	- note we are doing this over the real numbers, for zero knowledge proofs we would use a finite field
- oracles
  write a contract with a view function to return the median price for BTC/USD
	- see [solution](https://github.com/jobez/CairoBootcamp/commit/b86aa3f112bf3b971fb03ed14ecc3d09238ead97)
	-