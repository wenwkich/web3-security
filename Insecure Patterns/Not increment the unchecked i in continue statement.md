#infinite-loop

The following code use `unchecked` pattern to save gas when increment a for loop, but might result in the infinite loop because in the clause where it continues, it didn't increment the `i`
```solidity
contract Loop {
	function find_odds(uint256[] calldata nums) {
		uint256[] memory newArr = new uint256[]();
		for (uint256 i = 0; i < nums.length; ) {
			if (nums[i] % 2 == 0) {
				continue;
			}

			newArr.push(nums[i]);

			unchecked {
				++i;
			}
		}
		return newArr;
	}
}
```


Correct code:
```solidity
contract Loop {
	function find_odds(uint256[] calldata nums) {
		uint256[] memory newArr = new uint256[]();
		for (uint256 i = 0; i < nums.length; ) {
			if (nums[i] % 2 == 0) {
				unchecked {
					++i;
				}
				continue;
			}

			newArr.push(nums[i]);

			unchecked {
				++i;
			}
		}
		return newArr;
	}
}
```