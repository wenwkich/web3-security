```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.1 <0.9.0;

contract SimpleAuction {
    function bid() public payable returns (uint256) { // Function
        // ...
    }
}
```

Visibility:
- `public` - any contract and account can call
- `private` - only inside the contract that defines the function
- `internal`- only inside contract that inherits an `internal` function
- `external` - only other contracts and accounts can call

Getter function can be declared:
- `view` function declares that no state will be changed
- `pure` function declares that no state variable will be changed or read

Also a function can be declared [[Payable]]
There are also special function like [[Fallback]]

Check [[Inheritance]] for function overriding

Every function has a [[Function Selector]], that can be used in abi encoding