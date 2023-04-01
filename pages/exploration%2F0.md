topic:: [[security]]

- inciting tweet discussion
  collapsed:: true
	- ELI5: how exactly do EVM L2s “inherit” security of ETH L1?
	  
	  Isn’t there still the same point of failures as using another L1 by bridging? 
	  
	  Concretely, an L2’s bridge + rollup logic are exposed to the same class of bugs/security flaws, no? Mar 26th, 2023, [[Richard | Tensor 📈⚡]]
	- @0xrwu They don’t inherit security for the user.  A user running an L1 node can’t trust their coins in an L2 without running an L2 node. What they provide is a protocol for the L1 state machine to accept state transitions in the L2. Either with a zkp or a challenge timeout.
	  
	  L2 security… https://t.co/ajs6Hp4yJs Mar 26th, 2023, [[toly 🇺🇸]]
		- L2 security is still equal to the number of full nodes executing the L2 state transitions and confirming that execution is correct.
	- @aeyakovenko @0xrwu What? They do inherit certain security guarantees. A user running an L1 full node can be assured that their coins are secure in an L2 as long as [[security assumptions of the STF correctness enforcement]] mechanism hold.
	  
	  The L2 security part is not true either. Mar 26th, 2023, [[Toghrul Maharramov📜 🇺🇦]]
	- @toghrulmaharram @0xrwu How does that user know there are any honest full nodes evaluating the L2? Mar 26th, 2023, [[toly 🇺🇸]]
	- @aeyakovenko @0xrwu If you run an L1 full node and assume that the security of the proof mechanism holds, then, you shouldn't care about others evaluating the L2. Mar 26th, 2023, [[Toghrul Maharramov📜 🇺🇦]]
	- @toghrulmaharram @0xrwu If there are no honest L2 full nodes to issue a fraud proof then your coins are not safe. It’s the same thing as holding coins in the L1 without running an L1 node. Mar 26th, 2023, [[toly 🇺🇸]]
	- @aeyakovenko @0xrwu This is enshrined into the "security of the proof mechanism holds" part. Also, honesty assumptions are not applicable to zkRUs. Mar 26th, 2023, [[Toghrul Maharramov📜 🇺🇦]]
	- @aeyakovenko @0xrwu Then the security assumption of the proof mechanism has failed. Mar 26th, 2023, [[Toghrul Maharramov📜 🇺🇦]]
	- @toghrulmaharram @0xrwu Even less guarantees if the rollup runs its own bft and gives users confirmations faster then the L1 finality. Mar 26th, 2023, [[toly 🇺🇸]]
	- @aeyakovenko @0xrwu Yeah, if you rely on confirmation and wait for finality, you need to run an L2 full node. Mar 26th, 2023, [[Toghrul Maharramov📜 🇺🇦]]
		- dont wait*
	- @aeyakovenko @0xrwu I’ve been shouting similar sentiments from the rooftops for quite a while now. I don’t want L2 security issues to be wholly overlooked because everyone assumes that security is simply inherited from ETH and that’s the end of the story. Mar 26th, 2023, [[permaweave.Ξ◎ⓐ]]
- [[chatgpt/4]] summary
  collapsed:: true
	- In this Twitter discussion, participants are debating the security aspects of Ethereum Layer 2 ([[layer 2]]) solutions and how they inherit security from Ethereum Layer 1 ([[layer 1]]).
	- They are discussing the possible points of failure and the role of the users in maintaining the security of their assets on L2.
	- Richard | Tensor 📈⚡ raises the question of how L2 solutions inherit the security of L1, pointing out that the bridge and rollup logic could be exposed to the same class of bugs and security flaws as using another L1.
	- toly 🇺🇸 argues that users running an L1 node can't trust their coins in an L2 without running an L2 node, and that L2 security depends on the number of full nodes executing the L2 state transitions and confirming their correctness.
	- Toghrul Maharramov disagrees with toly, stating that L2s do inherit certain security guarantees from L1, as long as the security assumptions of the STF (State Transition Function) correctness enforcement mechanism hold.
		- He also disagrees with toly's assertion about the L2 security being dependent on the number of full nodes.
	- toly 🇺🇸 questions how a user running an L1 node can be sure there are any honest full nodes evaluating the L2.
	- Toghrul Maharramov📜 🇺🇦 responds by saying that if a user runs an L1 full node and trusts the security of the proof mechanism, they shouldn't care about others evaluating the L2.
	- toly 🇺🇸 argues that if there are no honest L2 full nodes to issue a fraud proof, the user's coins are not safe, comparing the situation to holding coins in the L1 without running an L1 node.
	- Toghrul Maharramov📜 🇺🇦 counters by emphasizing that the "security of the proof mechanism holds" part already covers this concern, and that honesty assumptions are not applicable to zkRUs (Zero-Knowledge Rollups).
	- toly 🇺🇸 brings up the issue of rollups running their own BFT (Byzantine Fault Tolerant) consensus algorithms, which could provide users with faster confirmations than L1 finality, potentially leading to even less security guarantees.
	- permaweave.Ξ◎ⓐ chimes in, expressing concern that L2 security issues may be overlooked due to the assumption that security is inherited from Ethereum L1 without any further considerations. They emphasize the importance of not taking the inherited security for granted and thoroughly examining the potential risks and vulnerabilities in L2 solutions.
	- summary
		- In summary, the participants in this discussion have differing views on how Ethereum L2 solutions inherit security from L1.
		- While some believe that L2s do inherit security guarantees as long as certain security assumptions hold, others argue that users must run both L1 and L2 nodes to ensure the security of their assets.
		- The conversation highlights the complexity of blockchain security and the importance of thoroughly understanding the potential risks and trade-offs in L2 solutions.
	-
- launch into chat w/ [[chatgpt/4]]
	- Q: Can you give a broad compare/contrast of how the ethereum ecosystem sees security in the 'layer' metaphor versus how let's say the [[cosmos]] ecosystem sees it? My understanding is cosmos has a broader category of potential ways blockchains can interact.
	  collapsed:: true
		- Is this a good understanding?
	- A: Yes, your understanding is generally accurate. Both Ethereum and Cosmos use the concept of layers to describe different levels of their ecosystems. However, the way they approach security and their layers' structure differs significantly.
	  collapsed:: true
		- [[Ethereum]]
			- In Ethereum, the layer metaphor is primarily used to describe the relationship between the base layer (Layer 1 or L1) and the various scaling solutions built on top of it (Layer 2 or L2).
			- Ethereum L1 is the main blockchain, where all transactions are secured and validated by the network's consensus mechanism, Proof of Work (being transitioned to Proof of Stake).
			- L2 solutions aim to improve scalability and reduce congestion on L1 by processing transactions off-chain and then settling the results back on L1.
			- Security in Ethereum's layer metaphor is based on the idea that L2 solutions inherit some security guarantees from L1.
			- The degree of inherited security depends on the specific L2 implementation (e.g., zkRollups, Optimistic Rollups) and the underlying cryptographic mechanisms.
			- In Ethereum, the primary focus is on ensuring the security of L1, which subsequently extends to L2 solutions.
		- [[cosmos]]
			- Cosmos takes a different approach, aiming to create an ecosystem of interconnected blockchains called [[zones]] .
			- Each zone is an independent blockchain with its consensus mechanism, which can be customized to the specific needs of the applications built on it.
			- Cosmos Hub is the central blockchain that connects all the zones and facilitates interoperability and communication between them using the Inter-Blockchain Communication (IBC) protocol.
			- In the Cosmos ecosystem, security is achieved through each zone's individual consensus mechanism, typically using Tendermint BFT (Byzantine Fault Tolerant) consensus.
			- The security of each zone is separate and does not directly depend on the security of the Cosmos Hub or other zones in the ecosystem.
			- The Cosmos Hub is responsible for facilitating cross-chain communication, but it does not provide or enforce security guarantees for the connected zones.
			- This approach enables greater flexibility and customization, allowing various blockchains to choose their own consensus mechanisms and validators to suit their specific use cases.
			- However, it also means that each zone must ensure its security, which could result in varying degrees of security across the entire Cosmos ecosystem.
		- comparison
			- In summary, Ethereum and Cosmos have different approaches to security in their layer metaphors.
			- Ethereum focuses on the security of the main blockchain (L1), with L2 solutions inheriting some security guarantees from it.
			- In contrast, Cosmos promotes an ecosystem of interconnected blockchains (zones) with independent security mechanisms, not relying on the Cosmos Hub to provide security.
			- Ethereum's approach results in a more unified security model, with L1's security extending to L2 solutions.
			- Meanwhile, Cosmos offers a more flexible architecture, allowing for a wide range of potential interactions between blockchains, but also requiring each zone to ensure its own security.
			- This flexibility enables the Cosmos ecosystem to accommodate various use cases and consensus mechanisms, but it also leads to a diverse range of security levels across the connected blockchains.
			- Both ecosystems have their strengths and weaknesses, and the choice between them largely depends on the specific needs and goals of the projects and applications built on these platforms.
			- Ethereum's security model is more focused on preserving the security of its base layer and extending it to L2 solutions, which is suitable for applications that require strong security guarantees.
			- On the other hand, Cosmos provides a more flexible and customizable environment for creating and connecting blockchains, which can be an advantage for projects that require greater adaptability and customization in terms of consensus mechanisms and interoperability.
	- Q: how does this relate to the idea of the state transition function?
	  collapsed:: true
		- it seems like the layer metaphor presumes a relationship of one state machines state transition function to another, whereas cosmos sees this presumption as coming at the cost of flexibility/scale.
		- so the zone itself being a blockchain that has some consumptive relationship to other blockchains seems to have a different view of the state transition function
	- A: The state transition function (STF) is a critical component in blockchain systems, as it defines the rules for transitioning from one state to another within a blockchain based on a set of transactions.
	  collapsed:: true
		- In the context of Ethereum and Cosmos, their different approaches to security and layering indeed have implications for the way they handle state transition functions.
		- [[Ethereum]]
			- In Ethereum, the layer metaphor implies a more hierarchical relationship between the state transition functions of L1 and L2.
			- The L1 STF describes the rules for processing and validating transactions on the Ethereum main chain, while L2 STFs define the rules for their respective scaling solutions.
			- L2 solutions in Ethereum are designed to interact with L1 in a way that they inherit some of its security properties.
			- This interaction is made possible by the STF of both layers being closely linked.
			- For instance, L2 solutions like zkRollups and Optimistic Rollups rely on cryptographic proofs and fraud proofs, respectively, to enforce the correctness of the L2 state transitions on L1.
			- As a result, the L2 STF is directly influenced and constrained by the L1 STF to ensure security and consistency across both layers.
		- [[cosmos]]
			- In Cosmos, the relationship between the state transition functions of different zones is more flexible and less hierarchical.
			- Each zone has its own blockchain with a unique state transition function, and the zones are connected to one another through the Cosmos Hub using the Inter-Blockchain Communication (IBC) protocol.
			- The IBC protocol enables the transfer of assets and data between zones without directly imposing constraints on their state transition functions.
				- This means that each zone can have a completely different STF, tailored to its specific needs and requirements.
			- The IBC protocol handles the communication and interoperability between zones by managing the exchange of messages and ensuring their correct execution.
				- In this context, Cosmos does not presume a strict relationship between state transition functions of different blockchains in the ecosystem.
				- Instead, it allows for a more flexible and adaptable approach, enabling a wide variety of state machines to interact and exchange information without directly affecting each other's STFs.
		- comparison
			- The key difference between the two approaches lies in the relationship between state transition functions in the respective ecosystems.
			- Ethereum's layer metaphor presumes a more direct and hierarchical relationship between L1 and L2 STFs, with L2 solutions inheriting some security properties from L1 through their closely linked state transition functions.
			- On the other hand, Cosmos focuses on enabling flexible and independent state transition functions across zones, with the IBC protocol providing a means for communication and interoperability between them.
			- In conclusion, Ethereum's layer metaphor places a stronger emphasis on the relationship between state transition functions to ensure security across L1 and L2, while Cosmos embraces a more flexible approach by allowing zones to have independent state transition functions and facilitating communication between them through the IBC protocol.
			- This flexibility comes at the cost of relinquishing direct control over the security properties of individual zones, which must be ensured by their own consensus mechanisms and state transition functions.
	- Q: It seems the different approaches view the scarcity of getting a critical mass of participants in a blockchain to secure it differently.
		- The l1 metaphor assumes it is the critical operating scarcity that defines the opportunity of a paradigm, right?
		- The l1 provides the singular monadic 'pulse' that consumes STF of other scaling solutions and subsumes other STF state transitions as succinct representations of its state in its own state. This is sort of a leviathan view, right?
		- Whereas cosmos sees the pulse as the possibility for dynamic interaction between a user, a domain, and its STF as encoded into a chain.
		- The chain itself can have dynamic interactions with other chains and one chain can consume info off of a group of other chains in a zone create a shared security assumption between them?
	- A: Yes, your interpretation of the two approaches is quite accurate.
	  collapsed:: true
		- The Ethereum and Cosmos ecosystems have different perspectives on the scarcity of securing a critical mass of participants in a blockchain, which is reflected in their design and architecture.
		- [[Ethereum]]
			- Ethereum's L1 metaphor indeed assumes that the security of the L1 blockchain, ensured by a critical mass of participants, is the crucial factor defining the opportunities and limitations of the ecosystem.
			- In this leviathan view, as you aptly describe, the L1 blockchain acts as the central authority, providing a secure and consistent foundation for other scaling solutions to build upon.
		- [[cosmos]]
			- Cosmos, on the other hand, takes a more decentralized and flexible approach by allowing multiple blockchains (zones) to coexist and interact dynamically.
			- The focus is on enabling user-specific, domain-specific, and STF-specific chains that can interact with other chains through the IBC protocol.
			- Each chain is responsible for securing its critical mass of participants and ensuring its security properties.
			- In this model, a group of chains within a zone can create shared security assumptions by combining their consensus mechanisms, validators, and economic incentives.
			- This allows them to create a more robust and secure environment for their users and applications.
			- However, it's important to note that the shared security is achieved within the group of chains in a zone and not directly enforced by the Cosmos Hub.
		- in summary
			- the Ethereum ecosystem emphasizes the importance of a secure and consistent foundation provided by the L1 blockchain, which then extends security to L2 solutions through the hierarchical relationship between their state transition functions.
			- This approach puts more weight on the scarcity of securing a critical mass of participants for L1, as it is seen as the key factor defining the opportunities and limitations of the entire ecosystem.
			- On the other hand, the Cosmos ecosystem embraces a more decentralized and flexible approach, where each chain within a zone is responsible for securing its critical mass of participants and ensuring its security properties.
			- Chains can interact dynamically with each other, enabling domain-specific and STF-specific chains to coexist and exchange information.
			- This approach allows for the creation of shared security assumptions between chains within a zone, fostering a more robust and secure environment for users and applications.
			- Ultimately, both approaches have their strengths and weaknesses, with Ethereum focusing on extending the security of L1 to L2 solutions and Cosmos promoting a more flexible and adaptable environment for creating and connecting blockchains with various consensus mechanisms and state transition functions.
			- The choice between these approaches depends on the specific needs and goals of the projects and applications built on these platforms.
	- S/Q: I think we can attempt to integrate these two views as ethereum having an implicitly conservative entry point take to a blockchain paradigm in the world and cosmos having a liberal dynamic view.
	  collapsed:: true
		- What I mean by that is I think cosmos presumes an order of magnitude more blockchain informed agencies that have the means to operate an instance of a chain and can coordinate on establishing their blockchain in shared security assumptions with other chains.
		- How long it takes for blockchain tech to become that endemic to human affairs where it becomes obvious that individuals would want the sovereignty of their own chain, which has an intrinsic complexity cost.
		- So what I am saying is, the cosmos model becomes valid if and only if you have a proportionally large amount of individuals who can 'pay' the intrinsic complexity cost, both in building a chain and interacting with multiple chains.
		- It has to be much greater than the amount of individuals who are willing to do so now, which is pretty maybe within 1-2 times the size of the people that are necessary to fulfill ethereums evolving ecosystem.
		- So maybe when its like 50 to 100 times the size, then the 'crisis' of how chains can DECIDE to choose shared security assumptions and decide to leave shared security assumptions makes sense, right?
	- A: Yes, your interpretation makes sense, and you've captured the essence of the two different views on blockchain adoption and their implications for security and complexity.
	  collapsed:: true
		- Ethereum takes a more conservative approach, focusing on a secure and consistent L1 foundation that extends security to L2 solutions, which is particularly suitable for the current state of blockchain adoption.
		- This approach acknowledges that there is a limited number of individuals with the necessary knowledge, resources, and willingness to participate in the ecosystem. It aims to optimize security and efficiency within this context.
		- Cosmos, on the other hand, envisions a more liberal and dynamic world where blockchain technology is widely adopted, and a significantly larger number of individuals and organizations can operate their own chains or participate in shared security assumptions.
		- This vision presumes that the benefits of blockchain technology will become increasingly clear, and the number of people willing to pay the complexity cost, both in terms of building and interacting with multiple chains, will grow substantially.
		- The Cosmos model becomes more relevant and advantageous as the number of blockchain-informed agencies increases, and the demand for customizability, flexibility, and interoperability grows.
		- When the size of the blockchain ecosystem reaches a critical mass (as you mentioned, perhaps 50 to 100 times the size of Ethereum's current ecosystem), the ability to decide on shared security assumptions and coordinate multiple chains with different security models and state transition functions becomes essential.
		- However, it's important to note that this is not a binary choice between Ethereum's conservative approach and Cosmos's liberal approach.
		- The future of blockchain technology may see the coexistence of both approaches, as they cater to different needs and applications within the broader ecosystem.
		- Ethereum's focus on security and consistency will continue to be valuable for applications that require strong security guarantees, while Cosmos's emphasis on flexibility and customizability will be beneficial for projects that require greater adaptability, interoperability, and support for diverse consensus mechanisms.
		- It is also possible that as blockchain technology matures and becomes more widespread, hybrid models that incorporate elements from both approaches could emerge.
		- These models could offer varying levels of security, customizability, and interoperability, allowing different projects and applications to select the optimal combination of features based on their specific requirements.
		- In conclusion, Ethereum's conservative approach and Cosmos's liberal approach represent different visions of the future of blockchain technology, with the former focusing on security and consistency for the current state of adoption and the latter envisioning a more dynamic and flexible ecosystem as blockchain technology becomes more pervasive.
		- As the blockchain ecosystem evolves, we may see the coexistence of both approaches, as well as the emergence of hybrid models that combine elements from each, providing a diverse range of options to cater to the varying needs of projects and applications in the space.
	- S/Q: Let's say that one of the expected dynamic changes / impacts that LLMs will have is that the way that we see the divide between theoretical and practical knowledge work gets maybe put into tigher coils / feedback loops.
		- Practical work will be able to be delegated to LLMs, freeing more people up to the question of theoretic possibilities.
		- I think that impact can change the complexity costs of a dev reasoning about constructing/running chains and for people to participate as a node.
		- So it may be that the speed that it takes to go to 50x the amount of skillful labor necessary to maintain ethereum happens a lot quicker, because the amount of human attention it took before the current recursive llm improvement cycle took off will be less than after.
		- Of course it is tempting to presume an n+1 recursion up and til we get some utopia, but the more likely outcome is that there will be some practical limitation requiring some theoretical reflection to guide, but that practical limitation will be about a stable resource that translates into individuals being able to delegate tasks to llms, and that delegation will impact the blockchain industry in some tangible way.
	- A: You raise an interesting point about the potential impact of large language models (LLMs) on the divide between theoretical and practical knowledge work, particularly in the context of the blockchain industry.
		- As LLMs advance and become more capable, it is plausible that they could take on an increasing amount of practical work, such as implementing, maintaining, and optimizing blockchain systems
		- . This shift would allow more human attention to be focused on exploring theoretical possibilities and addressing higher-level challenges.
		- In this scenario, the availability of skilled labor for blockchain development and maintenance could indeed increase more quickly than anticipated, as LLMs reduce the complexity costs associated with these tasks.
		- This could lead to a faster transition towards a more dynamic and flexible blockchain ecosystem, like the one envisioned by Cosmos.
		- The widespread adoption of LLMs for practical work might also facilitate the creation of new chains and shared security assumptions, as individuals and organizations can more easily reason about the intricacies of constructing and running chains.
		- However, as you rightly pointed out, it is important not to overestimate the potential of LLMs and expect a utopian outcome.
		- There will likely be practical limitations that will still require human intervention, particularly in addressing unforeseen challenges or developing novel solutions.
		- Moreover, the ethical implications of relying heavily on LLMs must also be considered, as well as potential issues related to centralization, security, and privacy.
		- In summary, the advancement of LLMs could significantly impact the blockchain industry by reducing complexity costs and enabling more people to participate in the development and maintenance of blockchain systems.
		- This could accelerate the growth of the blockchain ecosystem, leading to a more dynamic and flexible environment as envisioned by Cosmos, with a greater number of skilled individuals and organizations constructing, running, and securing chains.
		- However, it is crucial to remain cautious and consider the potential limitations and challenges associated with the widespread adoption of LLMs in the blockchain industry.
		- Practical limitations may still require human intervention, and theoretical reflection will remain essential for guiding the development of novel solutions and addressing unforeseen challenges.
		- Furthermore, the ethical, security, and privacy implications of relying on LLMs should not be overlooked.
		- It will be essential to strike a balance between leveraging the benefits of LLMs and ensuring that human expertise and ethical considerations continue to play a central role in the development and operation of blockchain systems
		- Overall, the relationship between LLMs and the blockchain industry is an exciting area for exploration, and the potential for synergy between these two domains could have far-reaching implications for the future of technology and society.