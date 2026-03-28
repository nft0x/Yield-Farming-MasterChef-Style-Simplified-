# Yield-Farming-MasterChef-Style-Simplified-
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract SimpleYieldFarm is Ownable {
    IERC20 public stakingToken;
    IERC20 public rewardToken;
    uint256 public rewardPerBlock;

    struct UserInfo {
        uint256 amount;
        uint256 rewardDebt;
    }

    mapping(address => UserInfo) public userInfo;

    constructor(address _stakingToken, address _rewardToken, uint256 _rewardPerBlock, address initialOwner)
        Ownable(initialOwner)
    {
        stakingToken = IERC20(_stakingToken);
        rewardToken = IERC20(_rewardToken);
        rewardPerBlock = _rewardPerBlock;
    }

    function deposit(uint256 amount) external {
        UserInfo storage user = userInfo[msg.sender];
        // Update reward logic simplified here
        stakingToken.transferFrom(msg.sender, address(this), amount);
        user.amount += amount;
    }

    function harvest() external {
        // Simplified reward calculation
        uint256 pending = userInfo[msg.sender].amount * rewardPerBlock; // demo
        rewardToken.transfer(msg.sender, pending);
    }
}
