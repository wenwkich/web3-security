`create` will create a new contract with account nonce, the address is unpredictable
```solidity
Car car = new Car(_owner, _model);
```

`create2` accepts `salt`, which will make contract address that can be precomputed
```solidity
Car car = (new Car){salt: _salt}(_owner, _model);
Car car = (new Car){value: msg.value, salt: _salt}(_owner, _model);
```