The value to mapping key `k` is located at `keccak256(h(k) . p)`, where
- `h` is a function that is applied to the key depending on its type
	* for value types, `h` pads the value to 32 bytes in the same way as when storing the value in memory.
	* for strings and byte arrays, `h(k)` is just the unpadded data.
- `.` is concatenation

e.g.

```solidity
contract C {
    struct S { uint16 a; uint16 b; uint256 c; }
    uint x;
    mapping(uint => mapping(uint => S)) data;
}
```

Location of `data[4][9].c`:
- the location of `data` is `1`
- `data[4]` is located at `keccak256(uint256(4) . uint256(1))`
- `data[4][9]` is located at `keccak256(uint256(9) . keccak256(uint256(4) . uint256(1)))`
- the slot offset inside the struct `S` is `1` because c is of type `uint256`, which will start at a new slot
- `data[4][9].c` is stored at `keccak256(uint256(9) . keccak256(uint256(4) . uint256(1))) + 1`