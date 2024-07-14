WETH will run `permit()` without error despite without implementing `IERC20Permit`, because it has a `fallback()` function
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import "./ERC20.sol";

contract WETH is ERC20 {
    event Deposit(address indexed account, uint256 amount);
    event Withdraw(address indexed account, uint256 amount);

    constructor() ERC20("Wrapped Ether", "WETH", 18) {}

    fallback() external payable {
        deposit();
    }

    function deposit() public payable {
        _mint(msg.sender, msg.value);
        emit Deposit(msg.sender, msg.value);
    }

    function withdraw(uint256 amount) external {
        _burn(msg.sender, amount);
        payable(msg.sender).transfer(amount);
        emit Withdraw(msg.sender, amount);
    }
}

```


In the following code, the attacker can attack with the following steps:
0. Alice gives infinite approval for `ERC20Bank` to spend `WETH`
1. Alice calls `deposit`, deposits 1 WETH into `ERC20Bank`
2. Attacker calls `depositWithPermit`, passes an empty signature and transfers all tokens from Alice into `ERC20Bank`, crediting the attacker for the deposit.
3. Attacker withdraws all tokens credited to him.
```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.24;

import "./IERC20Permit.sol";

contract ERC20Bank {
    IERC20Permit public immutable token;
    mapping(address => uint256) public balanceOf;

    constructor(address _token) {
        token = IERC20Permit(_token);
    }

    function deposit(uint256 _amount) external {
        token.transferFrom(msg.sender, address(this), _amount);
        balanceOf[msg.sender] += _amount;
    }

    function depositWithPermit(
        address owner,
        address recipient,
        uint256 amount,
        uint256 deadline,
        uint8 v,
        bytes32 r,
        bytes32 s
    ) external {
	    // this line will execute with WETH without any error
        token.permit(owner, address(this), amount, deadline, v, r, s);
        token.transferFrom(owner, address(this), amount);
        balanceOf[recipient] += amount;
    }

    function withdraw(uint256 _amount) external {
        balanceOf[msg.sender] -= _amount;
        token.transfer(msg.sender, _amount);
    }
}

```
