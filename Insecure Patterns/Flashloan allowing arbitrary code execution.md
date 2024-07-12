#loss-of-fund 

[[Flashloan]] shouldn't allow execution of arbitrary code execution, which could result in loss in fund

```solidity
contract Pool is ReentrancyGuard {

    using Address for address;

    IERC20 public immutable myToken;

    constructor (address tokenAddress) {
        myToken = IERC20(tokenAddress);
    }

    function flashLoan(
        uint256 borrowAmount,
        address borrower,
        address target,
        bytes calldata data
    )
        external
        nonReentrant
    {
        uint256 balanceBefore = myToken.balanceOf(address(this));
        require(balanceBefore >= borrowAmount, "Not enough tokens in pool");
        
        myToken.transfer(borrower, borrowAmount);
        target.functionCall(data);

        uint256 balanceAfter = myToken.balanceOf(address(this));
        require(balanceAfter >= balanceBefore, "Flash loan hasn't been paid back");
    }

}
```

Attacker can pass in any token as the target, and make the pool call `approve()` to steal the pool's token afterwards

Solution: use an interface to wrap the target address, only allow calling specific function only