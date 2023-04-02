- is built on [[zkp rollups]] architecture
	- an l2 scaling solution in which all funds are held by a smart contract on the mainchain
		- while computation and storage are performed off-chain
- for every rollup block
	- a state transition zero knowledge proof is generated and verified by the mainchain contract
- this snark includes the proof of the validity of every single transaction in the rollup block
	- additionally the public data update for every block is published over the mainchain network in the cheap call data
- hyperscaling
	- fractal-linke instances of [[zkevm]] running in parallel and with the common settlement on the l1 mainnet
	- the basechain is the mainchain instance of [[zksync/era]]
	- it serves as the default computational layer for generic smart contracts and as a settlement layer for all other hyperchains
- full node
	- executes zkevm bytecode using the viritual machine
	- filters incorrect transactions
	- executes mempool transactions
	- builds blocks
- [[prover]]
	- generates zk proofs from block witnesses
	- provides an interface for parallel proof generation
	- scalable (can increase # of provers depending on demand)
- interactor
	- the link between [[layer 1]] and layer2 zksync
	- calculates transaction fees
		- fees depend on token prices, proof generation, and l1 gas costs
- paranoid monitor