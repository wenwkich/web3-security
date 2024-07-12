Solidity supports multiple inheritance. Contracts can inherit other contract by using the `is` keyword.

Function that is going to be overridden by a child contract must be declared as `virtual`.

Function that is going to override a parent function must use the keyword `override`.

Full example in
https://solidity-by-example.org/inheritance/

```solidity
contract A { 
	function foo() public pure virtual returns (string memory) { 
		return "A"; 
	} 
} 

// Contracts inherit other contracts by using the keyword 'is'. 
contract B is A { 
	// Override A.foo() 
	function foo() public pure virtual override returns (string memory) { 
		return "B"; 
	} 
}
```

While functions can be overridden, state variables are not allowed to be overridden