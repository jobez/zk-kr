- #+BEGIN_QUOTE
  1. Try out the vanity bitcoin address example at asecurity or the Ethereum version
  #+END_QUOTE
- What do you understand by...
	- A problem can be viewed with respect requirements to its solution. 
	  id:: 63580ec4-2abb-415c-ac5f-cc1c9a4b39cb
	  The below notation is a way of expressing the time to find a solution, and how that time changes with with input n.
	- O(n), therefore, means that a problem's necessary time to solve for n scales linearly for the amount of inputs.
	- O(1) means that there is a constant time for the solution of a problem, regardless for the amount of inputs.
	- O(log n) means that the logarithm of input, which performs better than linear and possibly better than constant, depending on the duration of that constant time.
- Which of the above is best when describing proof size?
	- IF constant time is less than O(log n) for the amount of n you presume you particular usecase is, then the constant time algorithm. Otherwise, the log n.
- In your teams discuss why there are few identity projects on starknet
	- I presume this is because starknet uses zkp tech for scaleability instead of privacy. Identity is better suited for the utilization of zkp tech for privacy. I believe that you can get privacy 'on top of' starknet via a layer 3?
- install protostar