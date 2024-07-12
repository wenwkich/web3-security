Array slices are a view on a contiguous portion of an array. 
They are written as `x[start:end]`, where `start` and `end` are expressions resulting in a uint256 type (or implicitly convertible to it). 
The first element of the slice is `x[start]` and the last element is `x[end - 1]`

e.g.
```solidity
payload[:4]
```

