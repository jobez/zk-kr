- ((6428acdd-bea9-4eb2-85d9-676dd546e905))
	- we
		- aiming to create
			- [[arithmetic circuit]] and/or [[boolean circuit]] from our code
	- so
		- we
			- change
				- the [[high level language]]
					- into
						- a sequence of statements that are of two forms
							- and
								- `x = y` (where y can be a variable or a number)
								- `x = y (op) z`
									- (where op can be +, -, *, /) and y and z can be variables, numbers or themselves sub-expression
	- for example
		- we go
			- from
				- ```
				  def qeval(x):
				  	y = x**3 
				      return x + y + 5
				  ```
			- to [[arithmetic circuit]]
				- ```
				  sym_1 = x * x 
				  y = sym_1 * x 
				  sym_2 = y + x
				  ~out = sym_2 + 5
				  ```