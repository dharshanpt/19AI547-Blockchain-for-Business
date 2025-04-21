# Experiment 2: Blockchain-Based Crowdfunding (Kickstarter Alternative)
## Aim:
To create a decentralized crowdfunding platform where donors contribute funds only if the campaign goal is met.

## Algorithm:
1. Create Campaign – Deploy smart contract with goal, deadline, and details.
2. Contribute Funds – Backers send funds to the smart contract.
3. Check Outcome (After Deadline) –
4. If goal met → funds go to creator.
5. If goal not met → backers can refund.
6. Withdraw/Refund – Creator withdraws or backers claim refund.
7. Ensure Transparency – All actions are logged on blockchain.

## Program:
Developed by: Prem Kumar G
Register number: 21222323158
Date: 16/04/2025
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Crowdfunding {
    struct Campaign {
        address creator;
        uint256 goal;
        uint256 deadline;
        uint256 amountRaised;
        bool goalMet;
        mapping(address => uint256) contributions;
    }

    Campaign public campaign;

    constructor(uint256 _goal, uint256 _duration) {
        campaign.creator = msg.sender;
        campaign.goal = _goal;
        campaign.deadline = block.timestamp + _duration;
    }

    function contribute() public payable {
        require(block.timestamp < campaign.deadline, "Campaign ended");
        campaign.amountRaised += msg.value;
        campaign.contributions[msg.sender] += msg.value;
    }

    function withdrawFunds() public {
        require(msg.sender == campaign.creator, "Only creator can withdraw");
        require(campaign.amountRaised >= campaign.goal, "Goal not met");
        payable(msg.sender).transfer(campaign.amountRaised);
        campaign.goalMet = true;
    }

    function refund() public {
        require(block.timestamp > campaign.deadline, "Campaign still active");
        require(campaign.amountRaised < campaign.goal, "Goal was met");
        uint256 amount = campaign.contributions[msg.sender];
        campaign.contributions[msg.sender] = 0;
        payable(msg.sender).transfer(amount);
    }
}
```
# Expected Output:
Users can contribute ETH to the campaign.


If the goal is met, the creator can withdraw funds.


If the goal is not met, contributors can claim a refund.


# Output :
Contribute Output : 
![alt text](<contibute output.png>) 
Withdraw Output : 
![alt text](<withdrawl output.png>)
Campaign Output :
![alt text](<campaign output.png>)

# RESULT: 
   Thus, to create a decentralized crowdfunding platform where donors contribute funds only if the campaign goal is met is executed successfully.
