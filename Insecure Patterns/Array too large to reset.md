#array #exceed-block-limit

If the array in storage is too large, it will take more gas to reset the array that might exceed the block gas limit
Solidity array doesn't just reset the length, it also reset each value in it

See the following example
```solidity
contract Game {
	address[] private players;
	uint256 public gid = 0;
	mapping(uint256 => mapping(address => bool)) private gamePlayers;

	function play() external {
		require(!gamePlayers[gid][msg.sender], "already played");
		gamePlayers[gid][msg.sender] = true;
		players.push(msg.sender);
	}

	function newGame() external {
		gid += 1;
		players = new address[](0);
	}
}
```

Solution: use `mapping(gid => address[])` instead