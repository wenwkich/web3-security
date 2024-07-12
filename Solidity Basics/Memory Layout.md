https://docs.soliditylang.org/en/v0.8.26/internals/layout_in_memory.html

Solidity reserves four 32-byte slots, with specific byte ranges (inclusive of endpoints) being used as follows:

- `0x00` - `0x3f` (64 bytes): scratch space for hashing methods
	- Scratch space can be used between statements (i.e. within inline assembly)
- `0x40` - `0x5f` (32 bytes): currently allocated memory size (aka. free memory pointer)
- `0x60` - `0x7f` (32 bytes): zero slot
	- The zero slot is used as initial value for dynamic memory arrays
	- should never be written to
- the free memory pointer points to `0x80` initially

Elements in memory always occupies multiples of 32 bytes, even for `bytes1`
For example, the following array occupies 128 bytes in memory, but 32 bytes in storage
```solidity
uint8[4] a;
```

The following struct occupies 96 bytes (3 slots of 32 bytes) in storage, but 128 bytes (4 items with 32 bytes each) in memory
```solidity
struct S {
    uint a;
    uint b;
    uint8 c;
    uint8 d;
}
```