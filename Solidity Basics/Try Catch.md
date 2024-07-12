With external function call:
```solidity
try foo.myFunc(_i) returns (string memory result) { 
	emit Log(result); 
} catch { 
	emit Log("external call failed"); 
}
```

With contract creation:
```solidity
try new Foo(_owner) returns (Foo foo) { 
	// you can use variable foo here 
	emit Log("Foo created"); 
} catch Error(string memory reason) { 
	// catch failing revert() and require() 
	emit Log(reason); 
} catch (bytes memory reason) { 
	// catch failing assert() 
	emit LogBytes(reason); 
}
```