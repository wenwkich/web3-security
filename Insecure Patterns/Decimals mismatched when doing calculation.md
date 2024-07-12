#loss-of-fund

[[Decimals]]

When doing calculations of token amount, needs to pay cautious to their token decimals are matched or not

https://github.com/sherlock-audit/2024-06-symmetrical-update-2-judging/issues/5

```solidity
AccountStorage.layout().balances[bridgeLayout.invalidBridgedAmountsPool] += (bridgeTransaction.amount - validAmount);
```

The left hand side is of 18 decimals, but the right hand side is could be of 6 decimals (usdc)