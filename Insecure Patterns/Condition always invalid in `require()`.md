#DoS 

This usually appears in `require()` or conditions for `revert()`

https://github.com/sherlock-audit/2024-06-symmetrical-update-2-judging/issues/9

For this example, there's a line of code

```solidity
require(bridgeLayout.invalidBridgedAmountsPool != address(0), "BridgeFacet: Zero address");
```

The variable `bridgeLayout.invalidBridgedAmountsPool` is never modified in the project scope, hence the `require()` check will be always invalid