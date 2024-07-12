https://docs.soliditylang.org/en/v0.8.26/units-and-global-variables.html

The commonly used ones are 
- `msg.data` & `msg.value` and `msg.sender`
	- use `msg.sender` over `tx.origin`
- time units
- `abi.decode` & `abi.encode` (depending on use case, could be `abi.encodeWithXXX`)
- `require()` & `revert()`
	- `revert` vs `assert`
		- `revert` will refund the gas
		- `assert` don't refund the gas, failing assert means there's a bug
- `address(0x...).balance`
- `this` or `address(this)`