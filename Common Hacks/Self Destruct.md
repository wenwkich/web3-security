#DoS[[Denial of Service]]

https://solidity-by-example.org/hacks/self-destruct/

Self Destruct can force send eth to a contract (even without the payable `fallback()`)
this will make the following code not working properly

```solidity
uint256 balance = address(this).balance; 
require(balance <= targetAmount, "Game is over");
```

Solution:
Don't use `address(this).balance`, use a balance variable to track the balance instead