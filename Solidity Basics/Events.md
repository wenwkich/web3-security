```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.22;

event HighestBidIncreased(address bidder, uint amount); // Event

contract SimpleAuction {
    function bid() public payable {
        // ...
        emit HighestBidIncreased(msg.sender, msg.value); // Triggering event
    }
}
```

Events are not accessible with smart contract, but can be used by dapps for a event-driven design, advanced use case include:
- Event filtering and monitoring for real-time updates and analytics
- Event log analysis and decoding for data extraction and processing
- Event-driven architectures for decentralized applications (dApps)
- Event subscriptions for real-time notifications and updates