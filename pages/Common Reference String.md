- ((6429c756-8499-4909-bca3-a0c28fd214ad))
	- Please generate a dialogue between a researcher and decider about the best way to explain  and expand  the below excerpt then represent the expanded explanation in diagram form, using mermaid syntax:
		- Excerpt: ((6429c7ec-5cbb-4d35-bfc5-fa8965309dec))
			- ```mermaid
			  graph TD
			  A[CRS Model] --> B[Setup Phase]
			  B --> C[Randomized Process]
			  C --> D[CRS Construction]
			  D --> E[Broadcast CRS]
			  E --> F[Proof Construction]
			  F --> G[Proof Verification]
			  G --> H[Zero-Knowledge Proof]
			  
			  ``` #gpt/4
- ((6429cd48-6ba3-4da0-90e7-bfc5dbee9908))
	- victor is sending challenges to peggy
		- if
			- peggy
				- could know
					- what exactly
						- the challenge is going to be
			- then
				- she
					- could choose its [[randomness]]
						- in such a way
							- that
								- it
									- could satisfy
										- the challenge
			- even if
				- she did not know the correct solution for the instance
					- [[false proof]]
		- so victor must only issue the challenge after Peggy has fixed her [[randomness]]
		- this is why peggy first [[commits]] to he randomness, and implicitly reveals it only after the challenge, when she uses that value to commpute the proof
			- this ensures two things
				- victor cannot [[guess]] what value peggy commited to
				- peggy cannot change the value she committed to.