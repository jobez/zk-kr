file:: [how_and_why_snarks_work_1680470685704_0.pdf](../assets/how_and_why_snarks_work_1680470685704_0.pdf)
file-path:: ../assets/how_and_why_snarks_work_1680470685704_0.pdf

- if we have two non-equal polynomials of degree at most d, they can intersect at no more than d points.
  ls-type:: annotation
  hl-page:: 6
  hl-color:: green
  id:: 6429fa4e-8f15-4654-9911-779e89288e07
- This property flows from the method of finding shared points
  ls-type:: annotation
  hl-page:: 6
  hl-color:: blue
  id:: 6429fc6d-1e36-48d7-bdff-119bb9228a7a
- f we want to find intersections of two polynomials, we need to equate them. 
  ls-type:: annotation
  hl-page:: 6
  hl-color:: purple
  id:: 6429fcaf-569c-4981-9d00-8d57d635d8c1
- Hence we can conclude that evaluation6 of any polynomial at an arbitrary point is akin to the representation of its unique identity. 
  ls-type:: annotation
  hl-page:: 7
  hl-color:: green
  id:: 6429fd4e-a226-461f-8b1e-2368147ce22a
- That is why if a prover claims to know some polynomial (no matter how large its degree is) that the verifier also knows, they can follow a simple protocol to verify the statement
  ls-type:: annotation
  hl-page:: 7
  hl-color:: blue
  id:: 6429fe54-48a2-4491-914b-599b2dc7f62c
- 2 The Medium of a Proof
  ls-type:: annotation
  hl-page:: 5
  hl-color:: purple
  id:: 642a0477-a34f-4cb8-96d3-f2e5ce233731
- why polynomials are at the very core of zk-SNARK, although it is likely that other proof mediums exist as well.
  ls-type:: annotation
  hl-page:: 8
  hl-color:: green
  id:: 642a074b-ad20-436a-9c13-3df59e640f1a
- If we, for example, consider an integer range of x from 1 to 1077, the number of points where evaluations are different is 1077 − d. Henceforth the probability that x accidentally “hits” any of the d shared points is equal to d1077 , which is considered negligible.
  ls-type:: annotation
  hl-page:: 7
  hl-color:: green
  id:: 642a10a0-ccb0-4395-b78d-449ec026c27f
- 3 Non-Interactive Zero-Knowledge of a Polynomial
  ls-type:: annotation
  hl-page:: 8
  hl-color:: blue
  id:: 642a10f7-dd73-4c72-bd0d-451fdbbf1400
- 3.3.1 Homomorphic Encryption
  ls-type:: annotation
  hl-page:: 11
  hl-color:: purple
  id:: 642ac6bc-2449-4fd1-a5cb-4426870a658c
- 3.3.2 Modular Arithmetic
  ls-type:: annotation
  hl-page:: 12
  hl-color:: green
  id:: 642ac864-8d08-4b64-a1d7-c556fb3a6a29
- So why on earth is that helpful? It turns out that if we use modulo arithmetic, having a result of operation it is non-trivial to go back to the original numbers because many different combinations will have the same result
  ls-type:: annotation
  hl-page:: 13
  hl-color:: blue
  id:: 642ac8f5-0a88-4008-804a-6d9be5572971
- 3.3.3 Strong Homomorphic Encryption
  ls-type:: annotation
  hl-page:: 14
  hl-color:: green
  id:: 642ac938-4a61-46f2-be59-c3211c808aa3
- In fact, if modulo is sufficiently large, it becomes infeasible to do so, and a good portion of the modern-day cryptography is based on the “hardness” of this problem.
  ls-type:: annotation
  hl-page:: 14
  hl-color:: blue
  id:: 642aca49-1f0e-4075-8f8e-c2de048b7a5f
- Let us explicitly state the encryption function: E(v) = gv (mod n), where v is the value we want to encrypt.
  ls-type:: annotation
  hl-page:: 14
  hl-color:: blue
  id:: 642acae8-f122-4a54-bc31-b29c53c7af69
- 3.3.4 Encrypted Polynomial
  ls-type:: annotation
  hl-page:: 14
  hl-color:: blue
  id:: 642acbc9-a4db-4fea-a01a-f7eeaf64261e
- As the result of such operations, we have an encrypted evaluation of our polynomial at some unknown to us x. This is quite a powerful mechanism, and because of the homomorphic property, the encrypted evaluations of the same polynomials are always the same in encrypted space.
  ls-type:: annotation
  hl-page:: 15
  hl-color:: yellow
  id:: 642ace37-a942-41c0-984b-6189184f8bf1
- While in such protocol the prover’s agility is limited he still can use any other means to forge a proof without actually using the provided encryptions of powers of s, for example, if the prover claims to have a satisfactory polynomial using only 2 powers s3 and s1, that is not possible to verify in the current protocol.
  ls-type:: annotation
  hl-page:: 15
  hl-color:: blue
  id:: 642aceef-6ac1-4573-b415-8c4c4954039b
- 3.4 Restricting a Polynomial
  ls-type:: annotation
  hl-page:: 16
  hl-color:: purple
  id:: 642acf21-3316-42c5-b8a4-665455c4502c
- why verifier needs the proof that only supplied encryptions of powers of s were used to calculate gp and gh and nothing else
  ls-type:: annotation
  hl-page:: 16
  hl-color:: blue
  id:: 642acf6c-3049-4093-8ecc-1cbf46c235b9
- A way to do this is to require to perform the same operation on another shifted encrypted value alongside with the original one, acting as an arithmetic analog of “checksum”, ensuring that the result is exponentiation of the original value.
  ls-type:: annotation
  hl-page:: 16
  hl-color:: green
  id:: 642acfbb-9253-4bf4-a22a-1b9478df8c47
- 3.5 Zero-Knowledge
  ls-type:: annotation
  hl-page:: 18
  hl-color:: blue
  id:: 642ad080-d269-479a-b790-a4337c9b700d
- The question is how do we alter the proof such that the checks still hold, but no knowledge can be extracted? One answer can be derived from the previous section: we can “shift” those values by some random number δ (delta), e.g., (gp)δ . Now, in order to extract the knowledge, one18 first needs to find δ which is considered infeasible. Moreover, such randomization is statistically indistinguishable from random.
  ls-type:: annotation
  hl-page:: 18
  hl-color:: blue
  id:: 642ad1c4-7067-4579-bd05-440a42b01660
- 3.6 Non-Interactivity
  ls-type:: annotation
  hl-page:: 19
  hl-color:: blue
  id:: 642b1c7e-86c7-465d-ac41-bbdf8432a78c
- 3.6.1 Multiplication of Encrypted Values
  ls-type:: annotation
  hl-page:: 20
  hl-color:: red
  id:: 642b31d0-c268-483a-ba4f-46556ba8b6fa
- Note: from first glance, such limitation must only impede a dependent functionality, ironically in the zk-SNARK case it is a paramount property on which security of the scheme holds, see remark 3.3.
  ls-type:: annotation
  hl-page:: 20
  hl-color:: blue
  id:: 642b34b1-a764-4234-b583-0348b9487b5c
- 3.6.2 Trusted Party Setup
  ls-type:: annotation
  hl-page:: 21
  hl-color:: purple
  id:: 642b3afb-0790-4053-87dd-760b35541fed
- 3.6.3 Trusting One out of Many
  ls-type:: annotation
  hl-page:: 22
  hl-color:: yellow
  id:: 642b43de-393c-431a-b48a-07a60de0af02
- Here is an approach, let us consider three participants Alice, Bob and Carol with corresponding indices A, B and C, for i in 1, 2, . . . , d:
  ls-type:: annotation
  hl-page:: 22
  hl-color:: green
  id:: 642b454a-2ffb-4252-a4b9-7e6c4b2b7424
- 3.7 Succinct Non-Interactive Argument of Knowledge of Polynomial
  ls-type:: annotation
  hl-page:: 24
  hl-color:: blue
  id:: 642b45a2-3dc6-45a9-8f16-21c15d5399bd
- 3.7.1 Conclusions
  ls-type:: annotation
  hl-page:: 24
  hl-color:: red
  id:: 642b529c-e67b-4878-bac4-eb26f4d218c2
- 4 General-Purpose Zero-Knowledge Proofs
  ls-type:: annotation
  hl-page:: 25
  hl-color:: blue
  id:: 642b5365-ac49-4056-a7eb-fab62bf1cb73