`call` is a low level function to interact with other contracts.
This is the recommended method to use when you're just sending Ether via calling the `fallback` function.
However it is not the recommend way to call existing functions.

`delegatecall` is a low level function similar to `call`.
When contract `A` executes `delegatecall` to contract `B`, **`B`'s code is executed
with contract `A`'s storage, `msg.sender` and `msg.value`.**
