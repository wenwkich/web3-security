`delete a` assigns the default value to `a`
`delete a[x]` delete items at index `x` and leaves other elements and the length untouched
If `a` is a struct, this will assign a struct with all members reset
If `a` is a mapping, `delete a` will have no effects, `delete a[x]` will delete the value stored at `x`
If `a` is an array, `delete a` will reset all its member and its length to 0

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.4.0 <0.9.0;

contract DeleteExample {
    uint data;
    uint[] dataArray;

    function f() public {
        uint x = data;
        delete x; // sets x to 0, does not affect data
        delete data; // sets data to 0, does not affect x
        uint[] storage y = dataArray;
        delete dataArray; // this sets dataArray.length to zero, but as uint[] is a complex object, also
        // y is affected which is an alias to the storage object
        // On the other hand: "delete y" is not valid, as assignments to local variables
        // referencing storage objects can only be made from existing storage objects.
        assert(y.length == 0);
    }
}
```