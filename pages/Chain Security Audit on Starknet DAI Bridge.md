- maker dao implements
	- a layer 2 DAI contract for starknet
	  id:: 63726db3-15d7-4731-baf5-80a82f08c0df
	- dai bridging contracts from the layer 1 to layer 2
		- this includes contract for sending governance spells from layer 1 to layer 2
- the most critical subjects covered in our audit
	- the functional correctness and security of the DAI bridging mechanism
	- the functional correctness of the L2-DAI erc-20 contract
	- the protection against censorship
	- the functional correctness of relaying governance spells
- important to note that security audits are
	- time-boxed
	- cannot uncover all vulnerabilities
	- complement but don't replace other vital measures to secure a project
	- cannot eliminate the experimental nature intrinsic to an l2 solution, some risks will remain
- system overview
	- set of solidity and cairo contracts implement a bridge for DAI from Ethereum on layer 1 to the Starknet layer 2 solution and vice versa
	- following contracts are deployed
		- on l1
			- L1DAIBridge
			  id:: 63727002-cb22-46b6-b4f9-845cb443b043
			- L1Escrow
			  id:: 63727058-3397-4c43-bbb2-28b1b7673789
			- L1GovernanceRelay
		- on L2
			- dai
			  id:: 63727064-547f-4509-8d72-95ba28dbb3e3
			- l2_dai_bridge
			  id:: 63727065-1532-4bc9-8a49-8f18b88a4e1f
			- l2_governance_relay
- ((63727064-547f-4509-8d72-95ba28dbb3e3))
  collapsed:: true
	- a dai contract with functionality similar to an [[ERC20]] token is deployed.
		- `transfer`, `transferFrom`, `approve`, `allowance`, `balanceOf`, `totalSupply`
		- `mint` function is used by ((63727065-1532-4bc9-8a49-8f18b88a4e1f)) to mint tokens after a deposit from l1
		- the `burn` function can be used by anyone to burn his tokens is used by the ((63727065-1532-4bc9-8a49-8f18b88a4e1f)) during withdrawl of DAI from l2 back to l1
		- in order to mitigate the approval problem of the default ERC20 approve function, the contract implements `increase_allowance` and `decrease_allowance`
		- a magic number used as amount during approvals works as truly unlimited approval, for callers with this [[magic number]] as approved amount the approved amount is not reduced
		- on starknet, transactions sent into the network do not have a defined sender
			- external parties sending transactions into the network cannot be identified by an address
			- hence in order to hold tokens on starknet
				- each user must deploy a contract which has a unique address
				- such an address can then be used in the token contract to hold a balance and/or privilege role
- bridge ((63727002-cb22-46b6-b4f9-845cb443b043)) / ((63727065-1532-4bc9-8a49-8f18b88a4e1f))
	- the ((63727002-cb22-46b6-b4f9-845cb443b043)) contract implements all logic to deposit and withdraw DAI to and from L2
	- The deposited DAI are held by the ((63727058-3397-4c43-bbb2-28b1b7673789))