#steal-user-fund

https://solidity-by-example.org/hacks/vault-inflation/

This contract is vulnerable to vault inflation attack, attack (user 0) could front run innocent user (user 1) like this:
1. User 0 deposit 1
2. User 0 donate `100 * 1e18` token (without using `deposit`)
3. User 1 deposit `100 * 1e18` tokens (but his share will be 0)
4. User 0 withdraws `200 * 1e18` tokens

Let's see how the values are updated in each step:
```
   | balance        | user 0 shares | user 1 shares | total supply |
1. |              1 |             1 |             0 |            1 |
2. | 100 * 1e18 + 1 |             1 |             0 |            1 |
3. | 200 * 1e18 + 1 |             1 |             0 |            1 |
4. |              0 |             1 |             0 |            1 |
why user 1 share is 0?
user 1 shares = amount * total supply / balance 
			  = 100 * 1e18 * 1 / 100 * 1e18 
			  = 0
```


```solidity
contract Vault {
	IERC20 public immutable token;
	uint256 public totalSupply;
	mapping(address => uint256) public balanceOf;

	constructor(address _token) {
		_token = IERC20(_token);
	}

	function _mint(address _to, uint256 _shares) private {
		totalSupply += _shares;
		balanceOf[_to] += _shares;
	}

	function _burn(address _from, uint256 _shares) private {
		totalSupply -= _shares;
		balanceOf[_to] -= _shares;
	}

	function deposit(uint256 amount) {
		uint256 shares;
		if (totalSupply == 0) {
			shares = amount;
		} else {
			shares = (amount * totalSupply) / token.balanceOf(address(this));
		}
		_mint(msg.sender, shares);
		token.transferFrom(msg.sender, address(this), amount);
	}

	function withdraw(uint256 shares) {
		uint256 amount = (shares * token.balanceOf(address(this))) / totalSupply;
		_burn(msg.sender, shares);
		token.transfer(msg.sender, amount);
		return amount;
	}
}
```

How to protect:
- Min shares -> protects from front running
- Internal balance -> protects from donation
- Dead shares -> contract is first depositor
- Decimal offset (OpenZeppelin ERC4626)