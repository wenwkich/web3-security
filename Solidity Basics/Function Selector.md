When a function is called, the first 4 bytes of `calldata` specifies which function to call.

This 4 bytes is called a function selector.

`transfer(address,uint256)` is the function signature
Which can be used like this
```solidity
addr.call(abi.encodeWithSignature("transfer(address,uint256)", 0xSomeAddress, 123))
```

To get the function selector:
```solidity
bytes4(keccak256(bytes(_func)))
```


