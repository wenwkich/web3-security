Three types of data location: `storage`, `memory` and `calldata`:
- `storage` will persist in the blockchain state
- `memory` is volatile, gets cleared outside of a function
	- used when modification is needed
- `calldata` is a special location of function argument
	- immutable
	- save more gas

## Assignment Behavior

- Assignments between `storage` and `memory` (or from `calldata`) always create an independent copy
- Assignments from `memory` to `memory` only create references. This means that changes to one memory variable are also visible in all other memory variables that refer to the same data
- Assignments from `storage` to a **local** storage variable also only assign a reference
	- You cannot `delete` a local storage variable
- All other assignments to `storage` always copy