- a typescript library for writing smart contracts
	- based on
		- [[zero knowledge proofs]] for [[Mina]]
- primitive data types
	- ```
	  new Bool(x); 
	  // accepts true or false 
	  new Field(x); // accepts an integer,or a numeric string 
	  new UInt64(x); // accepts a Field - useful for constraining numbers to 64 bits 
	  new UInt32(x); // accepts a Field - useful for constraining numbers to 32 bits 
	  PrivateKey, PublicKey, Signature; // useful for accounts and signing 
	  new Group(x, y); // a point on our elliptic curve, accepts two Fields/numbers/strings 
	  Scalar; // the corresponding scalar field (different than Field)
	  ```
- common methods
	- ```
	  let x = new Field(4); // x = 4 
	  x = x.add(3); // x = 7 
	  let hash = Poseidon.hash([x]); // takes array of Fields, returns Field 
	  x = x.sub(1); // x = 6 
	  let privKey = PrivateKey.random(); // create a private key 
	  x = x.mul(3); // x = 18 
	  let pubKey = PublicKey.fromPrivateKey(privKey); // derive public key 
	  x = x.div(2); // x = 9 
	  let msg = [hash]; 
	  x = x.square(); // x = 81 
	  let sig = Signature.create(privKey, msg); // sign a message 
	  x = x.sqrt(); // x = 9 
	  let b = x.equals(8); // b = Bool(false) 
	  b = x.greaterThan(8); // b = Bool(true) 
	  b = b.not().or(b).and(b); // b = Bool(true) 
	  b.toBoolean(); // true 
	  sig.verify(pubKey, msg); // Bool(true)
	  ```