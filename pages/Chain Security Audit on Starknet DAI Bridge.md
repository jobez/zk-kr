- maker dao implements
  collapsed:: true
	- a layer 2 DAI contract for starknet
	  id:: 63726db3-15d7-4731-baf5-80a82f08c0df
	- dai bridging contracts from the layer 1 to layer 2
		- this includes contract for sending governance spells from layer 1 to layer 2
- the most critical subjects covered in our audit
  collapsed:: true
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
			  id:: 6372705c-0cb3-4838-a6f1-699fc801260a
		- on L2
			- dai
			  id:: 63727064-547f-4509-8d72-95ba28dbb3e3
			- l2_dai_bridge
			  id:: 63727065-1532-4bc9-8a49-8f18b88a4e1f
			- l2_governance_relay
			  id:: 63727069-aa48-484f-841f-8600b924c50a
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
	  collapsed:: true
		- the ((63727002-cb22-46b6-b4f9-845cb443b043)) contract implements all logic to deposit and withdraw DAI to and from L2
		- The deposited DAI are held by the ((63727058-3397-4c43-bbb2-28b1b7673789)) contract
			- the separation of funds from the logic makes the system upgradable -- l1 bridge can be replaced independent of escrow
		- l2 bridge contract
			- handles deposit and withdrawals
				- escrow is not needed on l2
				  id:: 6372ab54-0ef8-4393-893d-0d12bb32cf49
			- can be upgraded through a governance spell through the ((63727069-aa48-484f-841f-8600b924c50a)) contract that could remove the bridge's minting and burning rights and assign them a new bridge.
		- pausing the bridge
			- can be done independently on l1 and l2
			- once paused, it cannot be unpaused
			- pausing l1 bridge contract means deposits from l1 are no longer possible
			- pausing the l2 bridge contract prevents new withdrawals from being initiated
			- finalizing withdrawals on l1 that have been initiated before the l2bridge contract was paused remains possible even when the l1 bridge contract is paused
			- even when the l2 bridge contract is paused, deposit requests from l1 can still be finalized
			- the forced withdrawal functionality works when the l1 bridge is not paused and is independent of the pause state of the l2 bridge
		- depositing
			- contract has a ceiling limiting the total amount of DAI that can be deposited using this bridge
				- limits the impact of a potential bug on the core Maker system
				- limit can be changed while the system is life
			- depositing DAI from L1 works as follows
				- user approves the ((63727002-cb22-46b6-b4f9-845cb443b043)) contract to transfer the amount of DAI
				- next the user calls `deposit()` passing the amount, the source of the funds, and the destination address for the DAI on L2
				- after a period of blocks the Starknet bridge automatically processes the transaction on L2 and mints the DAI for the specified address
			- withdrawing the DAI from l2 works as follows
				- the user executes `withdraw()` on the ((63727065-1532-4bc9-8a49-8f18b88a4e1f)) contract
					- they specify the amount and the l1 address that will be able to claim the funds on l1
				- after a period of time the StarkNet bridge has made the message available for consumption on l1.
					- now the specified caller can call ((63727002-cb22-46b6-b4f9-845cb443b043)) `finalizeWithdrawal()` using the correct parameters and the destination where the DAIs should be transferred to
				- a user may have multiple unconsuumed withdrawals at the same time
			-
				-
		-
	- ((63727069-aa48-484f-841f-8600b924c50a))
	  collapsed:: true
		- the governance relay contract on l2 is a ward, a privileged account, in all l2 contracts of the system
		- the MakerDAO governance on l1 can decide to execute any action on L2 using this contract.
			- technically this works as follows
				- a new contract is deployed on l2 featuring code to execute
				- on l1 the governance decides to execute this action and the call is relayed via ((6372705c-0cb3-4838-a6f1-699fc801260a)) contract
				- On l2, this triggers the execution of the specified contracts code as [[delegatecall]] in the contract of the l2 governance relay
	- ForceWithdrawal
		- starknet is centralized
		- transactions on l2 are executed by a so called [[sequencer]] which may censor transactions on l2 or could become unavailable for other reasons
		- this may block users from transferring DAI on l2 or initiating withdrawl of the DAI to l1
		- the ((63727002-cb22-46b6-b4f9-845cb443b043)) contract features a `forceWithdrawal` function
		- this function documents the withdrawal attempt on L1 and attempts to force withdraw the DAI from L2
		- the sequencer may or may not execute this operation on L2
			- its important to understand that the [[prover]] cannot prove a failed execution.
				- this means reverting transactions are indistinguishable from censored messages.
				- ((6372bd52-d09a-4a5b-8ecc-8f7ae1cf5792))
		- hence `finalize_force_withdrawal` must prevent all possible reverts (e.g.) due to insufficient balance/allowance and ensure the transaction terminates gracefully so it can be executed and proven.
			- either the withdrawal is executed or the failed attempt is visible on l1 which would document such a behavior of the sequencer and allow the DAO Governance to shut down the bridge and release the DAI of the users
		- the process of the force withdrawal may be described as follows
			- user initiates a force withdrawal on l1
			- a message to L1 is sent. the proof of sending the message is set in the starknet l1 contract.
				- note that failed consumption of a message can be monitored by the value set in 2
				- disallowing any reverts in the l1_handler in the l2 bridge contract allows for a distinguishment of censorship / system failure and failed messages
				- on l2, a registry contract exists which allows users on L2 to specify their address on L1 for the forced withdrawal functionality
					- for forced withdrawal to work successfully on L2 the user must have this address set as this is used for access control.
				- further more, should the MakerDAO governance assisted evacuation procedure be initiated as the Layer 2 solution starts censoring or becomes unavailable this registry is used to match L2 to L1 addresses during reimbursement.
					- Ideally, users register their l1 address before receiving DAI on l2
			- on proven consumption of the l1 message on l2, the message sent from l1 is marked as consumed
			- l2 sends a withdrawing message to l1
			- user can withdraw their funds
- trust model and roles
	- we assume the deployment and initialization (.e.g of the ward roles in order for ((63727002-cb22-46b6-b4f9-845cb443b043)) ) to be able to transfer DAI from the Escrow during withdrawal, for the ((63727065-1532-4bc9-8a49-8f18b88a4e1f)) to be able to mint DAI on L2 or the ((6372705c-0cb3-4838-a6f1-699fc801260a)) to be done correctly before the bridge becomes operational
	- users :: are fully untrusted
	- wards 
	  id:: 6372b8ab-cd49-4322-af2c-b475079fc189
	  Accounts holding the ward role in a contract of the maker system have access to all privileges functionality of this contract. As required the other contracts of the system and the Maker governance have the ward role in the contracts. These parties are assumed to act honestly and correctly at all times
	- Starkware
		- partially trusted
		- operates the bridge from l1 to l2
		- executes the L2 transactions
		- generally trusted to work without censorship, however, the smart contract implements a fallback should the partly censor transactions on l2 or become unavailable
		- registry
			- trusted external contract
			- note that the force withdraw mechanism relies on the security and correctness of this contract