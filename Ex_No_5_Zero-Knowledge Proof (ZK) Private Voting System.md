# Experiment 5: Zero-Knowledge Proof (ZK) Private Voting System
# Aim:
To implement a fully private and transparent voting system using Zero-Knowledge Proofs (ZKPs). This ensures that votes are counted fairly without revealing who voted for whom.

# Algorithm:

Step 1: Each voter creates a secret and makes a hidden (hashed) commitment of their vote.

Step 2: Voters register by submitting their hidden commitment to the smart contract.

Step 3: During the voting period, voters wait without revealing their actual vote.

Step 4: After voting ends, voters reveal their secret and choice to the contract.

Step 5: The contract checks if the revealed vote matches the original commitment and counts it.

Step 6: Finally, the total votes are announced without revealing who voted for whom.



# Program:

### Developed by: DHARSHAN PT
### Register number: 212223230046
### Date: 28/04/2025
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ZKVoting {
    struct Voter {
        bool registered;
        bytes32 voteCommitment;
    }

    mapping(address => Voter) public voters;
    uint256 public votesForA;
    uint256 public votesForB;

    event VoteCommitted(address voter, bytes32 commitment);
    event VoteRevealed(address voter, uint256 choice);

    function registerVoter(bytes32 commitment) public {
        require(!voters[msg.sender].registered, "Already registered");
        voters[msg.sender] = Voter(true, commitment);
        emit VoteCommitted(msg.sender, commitment);
    }

    function revealVote(string memory secret, uint256 choice) public {
        require(voters[msg.sender].registered, "Not registered");
        require(keccak256(abi.encodePacked(secret, choice)) == voters[msg.sender].voteCommitment, "Invalid proof");

        if (choice == 1) votesForA++;
        if (choice == 2) votesForB++;

        emit VoteRevealed(msg.sender, choice);
    }
}

```
# Expected Output:
Voters commit their votes privately.


When revealed, the contract verifies correctness but keeps votes anonymous.


Final result is publicly verifiable without exposing individual votes.



# High-Level Overview:
Uses ZKPs to ensure anonymous and fair elections.


Prevents vote tampering while maintaining voter privacy.


Mimics real-world ZK voting applications in governance and DAOs.

# Output:

### Register voter
![Screenshot 2025-04-28 132632](https://github.com/user-attachments/assets/ca60c99b-4683-411e-afe2-77c9940bf64d)



### Reveal vote
![Screenshot 2025-04-28 132708](https://github.com/user-attachments/assets/da20fdab-ae08-4510-8ed7-25b1bf51d4dd)

![Screenshot 2025-04-28 132904](https://github.com/user-attachments/assets/feb44ab2-f67f-4cf6-9216-7ef2b38b23fd)



### Register voter for new user

![Screenshot 2025-04-28 132936](https://github.com/user-attachments/assets/e8acd57b-27b4-4cbe-8e65-6ae579b37069)


### Reveal vote
![Screenshot 2025-04-28 133007](https://github.com/user-attachments/assets/a4315543-efca-4bc4-adeb-975daea5c22d)

![Screenshot 2025-04-28 133044](https://github.com/user-attachments/assets/20d58e02-4f06-4800-834d-2dde7a016dc8)


# RESULT: 

Thus, To implement a fully private and transparent voting system using Zero-Knowledge Proofs (ZKPs). This ensures that votes are counted fairly without revealing who voted for whom is successfully executed.
