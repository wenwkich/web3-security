Array data is stored starting from `keccak256(p)`, then the data are stored continuously like the static array (p is the location of the array, slot `p` stores the length of the dynamic array)

e.g. 
```solidity
contract {
	uint256 a; // slot 0
	uint256 b; // slot 1
	uint256[] arr; // slot 2
}
```

The data of `arr` will stored start from `keccak256(2)`, and slot 2 will store the length of the array
`arr[0]` will be stored in `keccak256(2)`
`arr[1]` will be stored in `keccak256(2) + 1`
and go on

For dynamic arrays of dynamic arrays, the rule applies recursively
e.g. `x[i][j]` , where type of x is `uint24[][]`
Assume x is stored at slot `p`, `x[i][j]` is stored at
```
keccak256(keccak256(p) + i) + floor(j / floor(256 / 24))
```
`keccak256(p)` is the position of first layer of array, i.e. `x[0]`
`keccak256(p) + i` is the position of `x[i]`, note that array type will always start at a new slot
`keccak256(keccak256(p) + i)` is the position of `x[i][0]`