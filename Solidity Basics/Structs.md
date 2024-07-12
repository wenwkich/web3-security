```solidity
struct Campaign {
	address payable beneficiary;
	uint fundingGoal;
	uint numFunders;
	uint amount;
	mapping(uint => Funder) funders;
}
```
