// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
contract Bridge {
    IERC20 public token;
    mapping(uint256 => bool) public lockedTokens;
    
    constructor(address _token) {
        token = IERC20(_token);
    }
    function lockTokens(uint256 amount) external {
        token.transferFrom(msg.sender, address(this), amount);
        lockedTokens[amount] = true;
    }
    function unlockTokens(uint256 amount, address targetChain) external {
        require(lockedTokens[amount], "Token not locked");
        lockedTokens[amount] = false;
        token.transfer(msg.sender, amount);
    }
}
