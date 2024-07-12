#ether-stuck

If the call to refund eth result isn't checked, extra eth could be stuck in the contract (because the call could fail if receiver is a contract didn't implement a payable fallback function)

```solidity
contract Exchange {

	function execute() {
		_execute();
		_returnDust();
	}
	
    function _returnDust() private {
        uint256 _remainingETH = remainingETH;
        assembly {
            if gt(_remainingETH, 0) {
                let callStatus := call(
                    gas(),
                    caller(),
                    selfbalance(),
                    0,
                    0,
                    0,
                    0
                )
            }
        }
    }
}
```

Solution:
Check the result of the call:
```solidity
function _returnDust() private {
    (bool success,) = payable(msg.sender).call{value: remainingETH}("");
    require(success, "ETH transfer failed");
}
```

