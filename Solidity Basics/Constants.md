Constants are variables that cannot be modified
Their value is hard coded (copied in every occurrence) and using constants can save gas cost

```solidity
// SPDX-License-Identifier: MIT 
pragma solidity ^0.8.24; 
contract Constants { 
	// coding convention to uppercase constant variables 
	address public constant MY_ADDRESS = 
		0x777788889999AaAAbBbbCcccddDdeeeEfFFfCcCc; 
	uint256 public constant MY_UINT = 123; 
}
```