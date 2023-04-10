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
- 4.1 Computation
  ls-type:: annotation
  hl-page:: 25
  hl-color:: green
  id:: 642c68fe-77e4-4796-8506-9fefba571f7f
- 4.2 Single Operation
  ls-type:: annotation
  hl-page:: 25
  hl-color:: green
  id:: 642c8155-3b43-4622-94ba-09df27f98dd0
- 4.2.1 Arithmetic Properties of Polynomials
  ls-type:: annotation
  hl-page:: 26
  hl-color:: green
  id:: 642c82b5-1939-484a-b498-d6123a4c37dc
- 4.3 Enforcing Operation
  ls-type:: annotation
  hl-page:: 27
  hl-color:: green
  id:: 642c869b-e36f-42e3-b253-49a55f49aa1e
- 4.4 Proof of Operation
  ls-type:: annotation
  hl-page:: 29
  hl-color:: green
  id:: 642ce2bf-3c1a-4200-977a-a48f1bd85617
- why the evaluations of polynomials l(s), r(s), o(s) have to be provided separately by the prove
  ls-type:: annotation
  hl-page:: 29
  hl-color:: green
  id:: 643189ba-a2ff-4f59-a26b-7c4f797f2878
- While the setup stage stays unchanged, here is the updated protocol:
  ls-type:: annotation
  hl-page:: 30
  hl-color:: red
  id:: 64318ebe-4364-4b1c-9b62-b0e49da191f7
- Such protocol allows to prove that the result of multiplication of two values is computed correctly.
  ls-type:: annotation
  hl-page:: 30
  hl-color:: purple
  id:: 64318f0c-c606-4040-a900-773401f6e9de
- 4.5 Multiple Operations
  ls-type:: annotation
  hl-page:: 30
  hl-color:: purple
  id:: 64318fb4-a6cf-4c56-b712-33b8ca5f4504
- 4.5.1 Polynomial Interpolation
  ls-type:: annotation
  hl-page:: 32
  hl-color:: green
  id:: 64319869-ee28-4988-bb95-13113ef0d4b1
- 4.5.2 Multi-Operation Polynomials
  ls-type:: annotation
  hl-page:: 33
  hl-color:: blue
  id:: 64319b01-dc74-430a-89ba-f10b07bc748f
- 4.6 Variable Polynomials
  ls-type:: annotation
  hl-page:: 35
  hl-color:: green
  id:: 64319e80-8312-4ca0-88ea-67313c65be90
- 4.6.1 Single-Variable Operand Polynomial
  ls-type:: annotation
  hl-page:: 36
  hl-color:: red
  id:: 6431ad10-31b4-4bad-8e20-7a95f5d3d1d6
- Remark 4.1
  ls-type:: annotation
  hl-page:: 38
  hl-color:: yellow
  id:: 6431b0a4-83aa-46f3-a84f-af8c2c5eafb5
- 4.9.3 Non-malleability of Variable and Variable Consistency Polynomials
  ls-type:: annotation
  hl-page:: 52
  hl-color:: yellow
  id:: 6431b10a-7350-49d3-a6a3-7f84eee97093
- 4.6.2 Multi-Variable Operand Polynomial
  ls-type:: annotation
  hl-page:: 38
  hl-color:: green
  id:: 6431b139-668f-4a29-9227-8d48f37d9e01
- 4.7 Construction Properties
  ls-type:: annotation
  hl-page:: 41
  hl-color:: green
  id:: 6431b1d8-45ec-4bc9-b80a-d806b7463ce7
- 4.7.1 Constant Coefficients
  ls-type:: annotation
  hl-page:: 41
  hl-color:: blue
  id:: 64321302-7e90-4334-a4b3-45799d73751f
- 4.7.2 Addition for Free
  ls-type:: annotation
  hl-page:: 42
  hl-color:: blue
  id:: 6432148b-0411-4a17-9adc-db4443006d89
- 4.7.3 Addition, Subtraction and Division
  ls-type:: annotation
  hl-page:: 43
  hl-color:: blue
  id:: 64321628-9d15-4d7a-ba0d-173e1e6d5807
- 4.8 Example Computation
  ls-type:: annotation
  hl-page:: 44
  hl-color:: green
  id:: 64321679-8384-4763-8386-91bdbc0d613a
- 4.9 Verifiable Computation Protocol
  ls-type:: annotation
  hl-page:: 48
  hl-color:: green
  id:: 6432173b-b052-415c-bf85-02eb04b6cf97
- 4.9.1 Non-Interchangeability of Operands and Output
  ls-type:: annotation
  hl-page:: 49
  hl-color:: green
  id:: 6432178d-b5d7-46bd-82da-debf23b52f32
- 4.9.2 Variable Consistency Across Operands
  ls-type:: annotation
  hl-page:: 50
  hl-color:: green
  id:: 64324a9f-ea78-4f71-8d79-788641fc4d00
- 4.9.4 Optimization of Variable Values Consistency Check
  ls-type:: annotation
  hl-page:: 54
  hl-color:: green
  id:: 64324c49-651a-436a-b1f6-f9056ae8b3e5
- 4.10 Constraints
  ls-type:: annotation
  hl-page:: 55
  hl-color:: green
  id:: 64324c64-9ab9-4338-841e-b68e387724f0
- Our analysis has been primarily focusing on the notion of operation. However, the protocol is not actually “computing” but rather is checking that the output value is the correct result of an operation for the operand’s values. That is why it is called a constraint, i.e., a verifier is constraining a prover to provide valid values for the predefined “program” no matter what are they. A multitude of constraints is called a constraint system (in our case it is a rank 1 constraint system or R1CS).
  ls-type:: annotation
  hl-page:: 55
  hl-color:: blue
  id:: 64324c8f-e74c-4381-9a2c-cc0e50685685
- 4.11 Public Inputs and One
  ls-type:: annotation
  hl-page:: 57
  hl-color:: green
  id:: 64324cfc-04df-4cb7-b467-dbc7aaf4e6df
- 4.12 Zero-Knowledge Proof of Computation
  ls-type:: annotation
  hl-page:: 58
  hl-color:: green
  id:: 64324d2b-173c-46b3-9d21-5645d38e5375
- 4.13 zk-SNARK Protocol
  ls-type:: annotation
  hl-page:: 61
  hl-color:: green
  id:: 64324d8a-ea53-4b3c-a23c-7440c80ef691