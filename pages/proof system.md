- ```mermaid
  graph TD
    A[Proof Systems] --> B[Interactive Proof Systems]
    B --> C[Zero-Knowledge Proof Systems]
  
    subgraph InteractiveProof
      IP1("Prover and verifier exchange messages")
      IP2("Convince verifier of statement's truth")
    end
  
    subgraph ZeroKnowledgeProof
      ZKP1("Prover convinces verifier without revealing additional information")
    end
  
    classDef proofSystem fill:#FFA726,stroke:#333,stroke-width:2px;
    classDef subProofSystem fill:#4CAF50,stroke:#333,stroke-width:2px;
    class A proofSystem;
    class B,C subProofSystem;
  
  ```