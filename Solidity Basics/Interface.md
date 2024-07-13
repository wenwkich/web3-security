You can interact with other contracts by declaring an `Interface`, see [[Calling other contract]]

Interface
- cannot have any functions implemented
- can inherit from other interfaces
- all declared functions must be `external`
- cannot declare a constructor
- cannot declare state variables

https://solidity-by-example.org/interface/
In this case, if you want to interact with uniswap contract, but you don't have the code, what can you do?

First, declare the uniswap interface
```solidity
interface UniswapV2Pair { 
	function getReserves() 
		external 
		view 
		returns (uint112 reserve0, uint112 reserve1, uint32 blockTimestampLast); 
}
```

Then you can call the contract like this:
```solidity
address pair = UniswapV2Factory(factory).getPair(dai, weth);
```