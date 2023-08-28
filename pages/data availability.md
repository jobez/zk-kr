- starknet currently operates in zkrollup mode [[validity rollup]]
	- this means that upon the acceptance of a state update on-chain, the state diff between the previous and new state is sent as [[calldata]] to [[Ethereum]]
	- this data is made available in the aims that a broader extent of individuals can take the role of reconstructing the state of starknet
- data can be made available
  id:: 636155a2-f2e9-410a-9d13-00f7a09bfda1
	- on chain (zk-rollup)
	- off chain (validium)
	- volition (hybrid of the above, offering a choice)
- in order to recreate the state of a system
	- transaction data is needed
- the data availability question is where this data is stored and how to make sure it is available to the participants in the system