You can send Ether to other contracts by
- `transfer` (2300 gas, throws error)
- `send` (2300 gas, returns bool)
- `call` (forward all gas or set gas, returns bool)

It's recommended to send ether with `call` with `nonReentrant` modifier