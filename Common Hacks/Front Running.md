https://solidity-by-example.org/hacks/front-running/

The following code is prone to front running, because the hacker can check the ethereum mempool, and sign the same msg with higher gas

Solution: use commit-reveal schemes, participants needs to commit their own solution with a solution hash (`commit(solutionHash)`). The solution hash will include participant address (using `msg.sender`) and the answers. Then the participant will call the `reveal(answers)` function to get the rewards, hackers cannot reveal the answer because `msg.sender` is included in the solution hash, hashing hacker's address will yield a different hash that doesn't match the solution hash the real winner committed. Check the link for the original solution.
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

contract FindThisHash {
    bytes32 public constant hash =
        0x564ccaf7594d66b1eaaea24fe01f0585bf52ee70852af4eac0cc4b04711cd0e2;

    constructor() payable {}

    function solve(string memory solution) public {
        require(
            hash == keccak256(abi.encodePacked(solution)), "Incorrect answer"
        );

        (bool sent,) = msg.sender.call{value: 10 ether}("");
        require(sent, "Failed to send Ether");
    }
}

```