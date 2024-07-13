- If `msg.data` is empty
	- Yes - check if there is a `receive()`function
		- yes - call `receive()`
		- no - call `fallback()`
	- Call `fallback()` depending on whether it's `payable` and caller sends ether or not
		- If `fallback` doesn't have `payable`, it is unable to receive ether
			- sending ether to the contract will revert
			- but one can force send ether using [[Self Destruct]]

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.6.2 <0.9.0;

contract Test {
    uint x;
    // This function is called for all messages sent to
    // this contract (there is no other function).
    // Sending Ether to this contract will cause an exception,
    // because the fallback function does not have the `payable`
    // modifier.
    fallback() external { x = 1; }
}

contract TestPayable {
    uint x;
    uint y;
    // This function is called for all messages sent to
    // this contract, except plain Ether transfers
    // (there is no other function except the receive function).
    // Any call with non-empty calldata to this contract will execute
    // the fallback function (even if Ether is sent along with the call).
    fallback() external payable { x = 1; y = msg.value; }

    // This function is called for plain Ether transfers, i.e.
    // for every call with empty calldata.
    receive() external payable { x = 2; y = msg.value; }
}
```

