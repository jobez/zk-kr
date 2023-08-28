file:: [main-moonmath_1690945392265_0.pdf](../assets/main-moonmath_1690945392265_0.pdf)
file-path:: ../assets/main-moonmath_1690945392265_0.pdf

- In the field of cryptography, zero-knowledge proofs or zero-knowledge protocols are a class of protocols that enable a party, known as the [[prover]] , to [[demonstrate the truth of a statement to other parties]] , referred to as verifiers, without revealing any information beyond [[the statement’s veracity]]
  hl-stamp:: 1690945805107
  hl-page:: 8
  ls-type:: annotation
  id:: 64c9c909-4127-4ed3-afe5-c377f4fa4ba0
  hl-color:: green
- This book is intended to provide a comprehensive introduction to the mathematical foundations and implementations of these [[proof systems]] , aimed at individuals with limited prior exposure to this area of research.
  hl-page:: 8
  ls-type:: annotation
  id:: 64cf84a3-befa-4e53-9cd4-d30a627c218b
  hl-color:: green
- Of particular significance in this context are zero-knowledge succinct, non-interactive arguments of knowledge (zk-SNARKs), which possess the advantage of being much smaller in size than the original data required to establish the truth of a statement, and verifiers can be conveyed through a single message from the prover.
  ls-type:: annotation
  hl-page:: 8
  hl-color:: red
  id:: 64cf8aad-b092-4b92-bdb2-9c19a7a26229
  hl-stamp:: 1691323055685
- CONTENTS CONTENTS7.3.2 Control Flow . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1877.3.2.1 The Conditional Assignment . . . . . . . . . . . . . . . . . 1877.3.2.2 Loops . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1907.3.3 Binary Field Representations . . . . . . . . . . . . . . . . . . . . . . . 1917.3.4 Cryptographic Primitives . . . . . . . . . . . . . . . . . . . . . . . . . 1927.3.4.1 Twisted Edwards curves . . . . . . . . . . . . . . . . . . . . 1927.3.4.1.1 Twisted Edwards curve constraints . . . . . . . . . 1927.3.4.1.2 Twisted Edwards curve addition . . . . . . . . . . 1938 Zero Knowledge Protocols 1958.1 Proof Systems . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1958.2 The “Groth16” Protocol . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1978.2.1 The Setup Phase . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 1988.2.2 The Prover Phase . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 2058.2.3 The Verification Phase . . . . . . . . . . . . . . . . . . . . . . . . . . 2108.2.4 Proof Simulation . . . . . . . . . . . . . . . . . . . . . . . . . . . . . 213 Index 216 Bibliography 220 License & Sideletter 222 vii Chapter 1 Introduction In the field of cryptography, zero-knowledge proofs or zero-knowledge protocols are a class of protocols that enable a party, known as the prover, to demonstrate the truth of a statement to other parties, referred to as verifiers, without revealing any information beyond the statement’s veracity. This book is intended to provide a comprehensive introduction to the mathematical foundations and implementations of these proof systems, aimed at individuals with limited prior exposure to this area of research. Of particular significance in this context are zero-knowledge succinct, non-interactive arguments of knowledge (zk-SNARKs), which possess the advantage of being much smaller in size than the original data required to establish the truth of a statement, and verifiers can be conveyed through a single message from the prover. From a practical standpoint, zk-SNARKs are intriguing because they allow for the honest computation of data to be proven publicly without disclosing the inputs to the computation, through the transmission of a concise transaction to a verifier embodied as a smart contract on a public blockchain. This facilitates the public verification of computation, improves the scalability of blockchain technology, and enhances the privacy of transactions.
  ls-type:: annotation
  hl-page:: 1
  hl-color:: red
  id:: 64cf8b61-a7d2-4b6e-aa08-062959bf2fe2