- Try using the remix plugin to deploy your storage contract to the test network.
	- has hilarious/tragic error arround newline characters from windows.
- look at open zeppelin libraries for accounts
- how does starknet's [[account abstraction]] differ than what is had in  Ethereum?
	- Accounts on starknet are definable as any smart contract, except it needs a cannonical entry point defined as `__execute__`
- add 'ownable' functionality to your storage contract using Open Zeppelin ownable library
	- see [diff](https://github.com/jobez/CairoBootcamp/commit/644119acc68e236958d79a0d4174d26ddf34a21e) and [usage](https://github.com/jobez/CairoBootcamp/commit/cdc68a958a69f4c983e95cd9c6180b5180412bb6)
	  id:: 6362a523-d0f7-499a-a55d-063002ce6282