- to process a deposit
  collapsed:: true
	- tc generates a random area of bytes
	- tc computes it through the [[pedersen hash]]
	- then send the token and the hash to the smart contract
	- the contract will then insert it into the [[merkel tree]]
	- the random bytes are known to the depositor and are used to form the [[commitment]] hash
- to process a withdrawal
  collapsed:: true
	- two pieces of info are used
	- secret random bytes and a nullifier
	- the same area of bytes is split into two separate parts
		- the secret on one side
		- the [[nullifier]] on the other side
			- the nullifier is hashed
		- the nullifier is a public input that is sent on-chain to get checked w/ the smart contract and the merkel tree data
		- it avoids double spending, for instance
- two zkps are provded
  collapsed:: true
	- to prove
		- the hash of the initial commitment
		- the nullifier
			- forms part of the commitment hash, so it cannot be changed by the spender
			- privacy is sustained as there is no way to link the hashed nullifier to the initial commitment
			- besides, even if the information that the transaction is present in the [[merkel root]], the information about the exact Merkle path, thus the location of the transaction, is kept private
			- de
	- wiithout releaving any information
- deposits
  collapsed:: true
	- are simple on a technological point of view
	- but expensive in terms of gas as they need to compute the hash and update the merkle tree
- withdrawals are complex, but cheaper as gas is only needed for the nullifier hash and the zkp
- thanks to a zk [[snarks]]
  collapsed:: true
	- it is possible to prove the hash of the initial commitment and of the nullifier without revealing any information
	- even if the nullifier is public, privacy is sustained as there is no way to link the hashed nullifier to the initial environment
	- even if some one has the information that the transaction is present in the merkel root, the information about the exact merkel path, thus the location of the transaction, is still kept private
	- deposits are simple on a technological point of view, but expensive in terms of gas as they need to compute the hash and update the Merkle tree
	- at the opposite end, the withdrawal process is complex, but cheaper as gas is only needed for the nullifier hash and the zkp
- https://betterprogramming.pub/understanding-zero-knowledge-proofs-through-the-source-code-of-tornado-cash-41d335c5475f
	- tornado cash is a coin mixer that you can use to anonymize your ethereum transactions
		- because of the logic of the blockchain, every transaction is public
		- if you have some ETH on your account, you cannot transfer it anonymously, because anybody can follow your transaction history on the blockchain
	- coin mixers, like tornado cash,
		- can solve this privacy problem by
			- breaking the on-chain link between the source and destination address by using zkp
	- if you want to anonymize one of your transactions, you have to deposite a small amount of eth (or erc20 token) on the tornado cash contract
	- after a little while, you can withdraw this 1 eth with a different account
	- the trick is that nobody can create a link between the depositor account and the withdrawal account
	- if hundreds of accounts deposit 1 eth on oneside and the other hundreds of accounts withdraw 1 eth on the other side, then nobody will be able to follow the path where the money moves
	- the technical challenge is that smart contract transactions are also public like any other transaction on the ethereum network.
		- this is the point where zkp will be relevant
	- when you deposite your 1 eth on the contract, you have to provide a [[commitment]]
		- this commitment is stored by the smart contract
	- when you withdraw 1 eth on the other side, you have to provide a [[nullifier]] and a zero knowledge proof
	- the [[nullifier]] is a unique ID that is in connection with the commitment and the ZKP proves the connection, but nobody knows which nullifier is assigned to which commitment (except the owner of the depositor/withdrawl account)
	- again, we can prove that one of the commitments is assigned to our [[nullifier]] without revealing our commitment
	- the nullifiers are tracked by the smart contract, so we can withdraw only one deposited eith with one nullifier
	- we have to understand the [[merkel tree]]
		- merkle trees are hash trees, where the leaves are the elements, and every node is a hash of the child roots
		- the root of the tree is the [[merkel root]], which represents the whole set of elements
			- if you add, remove, or change any element (leaf) in the tree, the merle root will change
			- the merkle root is a unique identifier of the set of elements. but how can we use it?
		- [[merkle proofs]] are available when one has a [[merkel root]]
			- you can send me a merkle proof that proves that an element is in the set represented by the root
			- example: [[whitelisting]]
				- imagine a smart contract that has a method that can be called by only whitelisted users
				- the porblem is that there are 1000 whitelisted accounts
				- how can you store them on the smart contract
				- the simple way is to store every account in mapping, but it is very expensive
				- a cheaper solution is building a merkle tree and storing the merkle root only
				- if somebody wants to call the method she has to give a merkle proof (in this case it is a list of 10 hashes)
			- **Again: A Merkle tree is used to represent a set of elements with one hash (the Merkle root). The existence of an element can be proven by the Merkle proof.**
		- The next thing that we have to understand is the zero-knowledge proof itself.
			- With ZKP, you can prove that you know something without revealing the thing that you know.
			- For generating a ZKP, you need a circuit.
			- A circuit is something like a small program that has public inputs and outputs, and private inputs.
			- These private inputs are the knowledge that you don’t reveal for the verification, this is why it is called zero-knowledge proof.
			- With ZKP, we can prove that the output can be generated from the inputs with the given circuit.
			- a simple circuit looks like this
				- ```
				  pragma circom 2.0.0;
				  
				  include "node_modules/circomlib/circuits/bitify.circom";
				  include "node_modules/circomlib/circuits/pedersen.circom";
				  
				  template Main() {
				      signal input nullifier;
				      signal output nullifierHash;
				  
				      component nullifierHasher = Pedersen(248);
				      component nullifierBits = Num2Bits(248);
				  
				      nullifierBits.in <== nullifier;
				      for (var i = 0; i < 248; i++) {
				          nullifierHasher.in[i] <== nullifierBits.out[i];
				      }
				  
				      nullifierHash <== nullifierHasher.out[0];
				  }
				  
				  component main = Main();
				  ```
		- Using this circuit, we can prove that we know the source of the given hash.
		- This circuit has one input (the nullifier) and one output (the nullifier hash). The default accessibility of the inputs is private, and the outputs are always public.
		- This circuit uses 2 libraries from the Circomlib. [Circomlib](https://github.com/iden3/circomlib) is a set of useful circuits.
		- The first library is bitlify that contains bit manipulation methods, and the second is pedersen which contains the Pedersen hasher.
		- Pedersen hashing is a hashing method that can be run efficiently in ZKP circuits.
		- In the body of the Main template, we fill the hasher and calculate the hash. (For more info about the circom language, please take a look at the [circom documentation](https://docs.circom.io/))
		- For generating the zero-knowledge proof, you will need a [[proving key]] .
		- This is the most sensitive part of ZKP because using the source data that is used to generate the proving key, anybody could generate fake proofs.
		- This source data is called the “ [[toxic waste]] ” that has to be dropped.
		- Because of this, there is a “ceremony” for generating the proving key.
		- The ceremony has many members and every member contribute to the proving key.
		- Only one non-malicious member is enough to generate a valid proving key.
		- Using the private inputs, the public inputs, and the proving key, the ZKP system can run the circuit and generate the Proof and the outputs.
		- ![](https://miro.medium.com/v2/resize:fit:527/0*jFHwERxrnWyuEBHZ.png)
		- ![](https://miro.medium.com/v2/resize:fit:707/0*SlVAKC2jfKq1c4Dk.png)
		- There is a validation key for the proving key that can be used for the validation. The validation system uses the public inputs, the outputs, and the validation key to validate the proof.
		- ![](https://miro.medium.com/v2/resize:fit:707/0*SlVAKC2jfKq1c4Dk.png)
		- Now, we have everything to understand how Tornado Cash (TC) works.
			- When you deposit 1 ETH on the TC contract, you have to provide a commitment hash.
			- This commitment hash will be stored in a Merkle tree.
			- When you withdraw this 1 ETH with a different account, you have to provide 2 zero-knowledge proofs.
				- The first proves that the Merkel tree contains your commitment.
				- This proof is a zero-knowledge proof of a Merkle proof.
					- But this is not enough, because you should be allowed to withdraw this 1 ETH only once.
					- Because of this, you have to provide a nullifier that is unique for the commitment.
					- The contract stores this nullifier, this ensures that you don’t be able to withdraw the deposited money more than one time.
		- The uniqueness of the nullifier is ensured by the commitment generation method.
		- The commitment is generated from the nullifier and a secret by hashing.
		- If you change the nullifier then the commitment will change, so one nullifier can be used for only one commitment.
		- Because of the one-way nature of hashing, it’s not possible to link the commitment and the nullifier, but we can generate a ZKP for it.
		- ![](https://miro.medium.com/v2/resize:fit:329/0*4L6nd2nGRKyd-b2w.png)
		- After the theory, let’s see how the [withdraw circuit of TC](https://github.com/tornadocash/tornado-core/blob/master/circuits/withdraw.circom) looks like:
			- ```
			  include "../node_modules/circomlib/circuits/bitify.circom";
			  include "../node_modules/circomlib/circuits/pedersen.circom";
			  include "merkleTree.circom";
			  // computes Pedersen(nullifier + secret)
			  template CommitmentHasher() {
			      signal input nullifier;
			      signal input secret;
			      signal output commitment;
			      signal output nullifierHash;
			      component commitmentHasher = Pedersen(496);
			      component nullifierHasher = Pedersen(248);
			      component nullifierBits = Num2Bits(248);
			      component secretBits = Num2Bits(248);
			      nullifierBits.in <== nullifier;
			      secretBits.in <== secret;
			      for (var i = 0; i < 248; i++) {
			          nullifierHasher.in[i] <== nullifierBits.out[i];
			          commitmentHasher.in[i] <== nullifierBits.out[i];
			          commitmentHasher.in[i + 248] <== secretBits.out[i];
			      }
			      commitment <== commitmentHasher.out[0];
			      nullifierHash <== nullifierHasher.out[0];
			  }
			  // Verifies that commitment that corresponds to given secret and nullifier is included in the merkle tree of deposits
			  template Withdraw(levels) {
			      signal input root;
			      signal input nullifierHash;
			      signal private input nullifier;
			      signal private input secret;
			      signal private input pathElements[levels];
			      signal private input pathIndices[levels];
			      component hasher = CommitmentHasher();
			      hasher.nullifier <== nullifier;
			      hasher.secret <== secret;
			      hasher.nullifierHash === nullifierHash;
			      component tree = MerkleTreeChecker(levels);
			      tree.leaf <== hasher.commitment;
			      tree.root <== root;
			      for (var i = 0; i < levels; i++) {
			          tree.pathElements[i] <== pathElements[i];
			          tree.pathIndices[i] <== pathIndices[i];
			      }
			  }
			  component main = Withdraw(20);
			  ```
		- The first template is the CommitmentHasher.
			- it has two inputs, the nullifier and the secret which are two random 248-bit numbers.
			- The template calculates the nullifier hash and the commitment hash which is a hash of the nullifier and the secret as I wrote before.
		- The second template is the Withdraw itself.
			- It has 2 public inputs, the Merkle root, and the nullifierHash.
			- The Merkle root is needed to verify the Merkle proof, and the nullifierHash is needed by the smart contract to store it.
			- The private input parameters are the nullifier, the secret, and the pathElements and pathIndices of the Merkle proof.
			- The circuit checks the nullifier by generating the commitment from it and from the secret and also checks the given Merkle proof.
			- If everything is fine, the zero-knowledge proof will be generated that can be validated by the TC smart contract.
		- You can find the smart contracts in the [contracts folder](https://github.com/tornadocash/tornado-core/tree/master/contracts) in the repo.
		- The Verifier is generated from the circuit.
		- It is used by the Tornado contract to verify the ZKP for the given nullifier hash and Merkle root.
		- The easiest way to use the contract is the [command-line interface](https://github.com/tornadocash/tornado-core/blob/master/src/cli.js).
		- It is written in JavaScript, and its source code is relatively simple. You can easily find where the parameters and the ZKP are generated and used to call the smart contract.
		- Zero-knowledge proof is relatively new in the crypto world. The math behind it is really complex and hard to understand, but tools like `snarkjs` and `circom` make it easy to use. I hope, this article helps you to understand this “magical” technology, and you can use ZKP in your next project.