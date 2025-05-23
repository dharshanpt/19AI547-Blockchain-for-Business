# Experiment 6: Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
# Aim:
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks.

# Algorithm:

Step 1: The user registers with their Ethereum address (public key) and a fake private key generated by the contract.

Step 2: The smart contract generates a random challenge message using the user’s address and the current timestamp.

Step 3: The user signs the challenge with their private key, creating a signature.

Step 4: The smart contract verifies the signature by checking it against the user's public key and the challenge.

Step 5: If the signature matches, the user is authenticated successfully.

Step 6: If the signature does not match, authentication fails, preventing unauthorized access.

# Program:

## Developed by: DHARSHAN PT
## Register number: 212223230046
## Date: 28/04/2025
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PasswordlessAuthDemo {
    struct User {
        bool registered;
        address pubKey;
        bytes32 privateKey; // Fake private key for demo
    }

    mapping(address => User) public users;
    bytes32 public latestChallenge;

    event UserRegistered(address user, address pubKey, bytes32 privateKey);
    event ChallengeGenerated(bytes32 challenge);
    event SignatureGenerated(bytes32 hash, uint8 v, bytes32 r, bytes32 s);

    // Step 1: Register user
    function registerUser() public {
        require(!users[msg.sender].registered, "Already registered");

        // Fake public/private keys
        address fakePubKey = msg.sender;
        bytes32 fakePrivateKey = keccak256(abi.encodePacked(msg.sender, block.timestamp));

        users[msg.sender] = User({
            registered: true,
            pubKey: fakePubKey,
            privateKey: fakePrivateKey
        });

        emit UserRegistered(msg.sender, fakePubKey, fakePrivateKey);
    }

    // Step 2: Generate random challenge
    function generateChallenge() public returns (bytes32) {
        require(users[msg.sender].registered, "User not registered");
        latestChallenge = keccak256(abi.encodePacked(block.timestamp, msg.sender));
        emit ChallengeGenerated(latestChallenge);
        return latestChallenge;
    }

    // Step 3: "Sign" the challenge (fake signing)
    function generateSignature() public returns (bytes32 hash, uint8 v, bytes32 r, bytes32 s) {
        require(users[msg.sender].registered, "User not registered");
        
        hash = latestChallenge;
        bytes32 combined = keccak256(abi.encodePacked(users[msg.sender].privateKey, hash));
        
        // Fake values for r, s, v
        r = bytes32(uint256(uint160(users[msg.sender].pubKey)) << 96);
        s = combined;
        v = 27;

        emit SignatureGenerated(hash, v, r, s);

        return (hash, v, r, s);
    }

    // Step 4: Authenticate
    function authenticate(bytes32 hash, uint8 v, bytes32 r, bytes32 s) public view returns (bool) {
        require(users[msg.sender].registered, "User not registered");

        bytes32 expectedCombined = keccak256(abi.encodePacked(users[msg.sender].privateKey, hash));
        bytes32 expectedR = bytes32(uint256(uint160(users[msg.sender].pubKey)) << 96);
        uint8 expectedV = 27;

        if (r == expectedR && s == expectedCombined && v == expectedV) {
            return true;
        } else {
            return false;
        }
    }
}

```

# Expected Output:
## Users can register without a password.
![WhatsApp Image 2025-04-28 at 15 00 11_42032937](https://github.com/user-attachments/assets/560eac53-d2b3-4c0a-9867-c0a54083ee98)


## Users sign a challenge with their private key for authentication.
![WhatsApp Image 2025-04-28 at 14 59 56_6d0c7089](https://github.com/user-attachments/assets/48cc7b9f-d6dd-47f3-9cb1-e9de3d5c5e31)


## The smart contract verifies signatures to confirm identity.
![WhatsApp Image 2025-04-28 at 15 00 01_ee46482d](https://github.com/user-attachments/assets/b5dda14e-b6f6-4e3d-aeef-0424eb3b7002)

![WhatsApp Image 2025-04-28 at 15 00 11_86cc48bf](https://github.com/user-attachments/assets/40ba54c1-69a9-432d-95cc-a70e17cb69cb)


# High-Level Overview:
Eliminates password hacks & phishing attacks.


Uses Ethereum's built-in cryptographic functions.


Inspired by Web3 login solutions like MetaMask authentication.




# RESULT: 

Thus, To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks is executed successfully.
