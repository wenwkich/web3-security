```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.0 <0.9.0;

contract Purchase {
    enum State { Created, Locked, Inactive } // Enum
}

// Returns uint 
// Created - 0 
// Locked - 1 
// Inactive - 2
```

Enum can be imported by other contract