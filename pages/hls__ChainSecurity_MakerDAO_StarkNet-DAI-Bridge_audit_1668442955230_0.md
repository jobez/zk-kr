file-path:: ../assets/ChainSecurity_MakerDAO_StarkNet-DAI-Bridge_audit_1668442955230_0.pdf
file:: [ChainSecurity_MakerDAO_StarkNet-DAI-Bridge_audit_1668442955230_0.pdf](../assets/ChainSecurity_MakerDAO_StarkNet-DAI-Bridge_audit_1668442955230_0.pdf)
title:: hls__ChainSecurity_MakerDAO_StarkNet-DAI-Bridge_audit_1668442955230_0

- MakerDAO implements a layer 2 DAI contract for StarkNet, a ZK-Rollup for Ethereum, and DAI bridging contracts from the layer 1 to layer 2. That also includes contracts for sending governance spells from layer 1 to layer 2.
  ls-type:: annotation
  hl-page:: 3
  id:: 63726da0-0258-4dcb-add9-7f4ce03b5af6
- It's important to understand why finalize_force_withdrawal must check whether the approval exists: Burning without the allowance would result in the transaction to revert. The prover can't prove failed executions, reverts are indistinguishable from censored messages. By checking the allowance and gracefully terminate the transaction when no sufficient allowance exist, the transaction can be executed. Hence the message from L1 can be processed which allows to clear the message in the StarkNet contract on Ethereum. This proves that the transaction must have been executed on L2.
  ls-type:: annotation
  hl-page:: 15
  id:: 6372bd52-d09a-4a5b-8ecc-8f7ae1cf5792