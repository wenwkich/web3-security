One way is [[Call and Delegatecall]], but it's usually not recommended (they have special use case)

Suppose there's an [[Interface]] or contract called `Callee` with the following function
```solidity
function setX(uint256 _x) public returns (uint256);
```

You can call the function like this
```solidity
Callee callee = Callee(_addr); 
callee.setX(_x);
```