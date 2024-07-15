- Data are stored in slots, which is 32 bytes in length
- Data is continuously stored in order starting with the first state variable, which is `slot 0`
- If multiple variables size right next to each other, and their size added up doesn't exceed 32 bytes, they are stored in the same slot 
	- for example, `uint240` and `bytes8` and `uint8` are stored in the same slot if they are next to each other, and if `uint240` starts on the fresh slot
- [[Structs]] and [[Arrays]] always start on a new slot
- Data following [[Structs]] and [[Arrays]] always start on a new slot
- Static arrays are stored continuously, while dynamic are not, see [[Storage Layout of Dynamic Array]]
- [[Storage Layout of Mappings]]

https://solidity-by-example.org/evm/storage/

Example
```solidity
contract EVM {
	// slot 0
	uint256 public full_slot;
	// slot 1
	// s_d | s_c | s_b | s_a
	// 32  | 32  | 64  | 128 bits
	uint128 public s_a; 
	uint64 public s_b; 
	uint32 public s_c; 
	uint32 public s_d;

	function test_sstore() public {
		assembly {
			// load 32 bytes from slot 1
			let v := sload(1) 
			
			// 000...1 shift left 128 then minus 1,
			// => 0000...111....1 (128 bits 1) 
			// then not it to get the first 128 bit
			// => 1111...000....0 (128 bits 0)
			let mask_a := not(sub(shl(128, 1), 1))
			
			// Set first 128 bits to 0 
			v := and(v, mask_a)
			
			// Set s_a = 11 
			v := or(v, 11)

			sstore(1, v)
		}
	}
}

```