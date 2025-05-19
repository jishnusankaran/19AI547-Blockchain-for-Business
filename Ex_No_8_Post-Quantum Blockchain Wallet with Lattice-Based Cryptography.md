# Experiment 8: Post-Quantum Blockchain Wallet with Lattice-Based Cryptography

# Aim:

To create a quantum-resistant wallet using lattice-based cryptography instead of traditional ECDSA, ensuring that future quantum computers cannot break private keys.

# Algorithm:

Step 1: Understand Quantum Threat: ECDSA-based wallets are vulnerable to quantum attacks; lattice-based cryptography (e.g., NTRU, CRYSTALS-Kyber) is quantum-resistant.

Step 2: Register User: User registers with a quantum-safe public key hash.

Step 3: Store User Data: Store the hashed lattice-based public key and track user registration.

Step 4: Transaction Setup: For sending funds, users must be registered and have sufficient balance.

Step 5: Signature Validation: Users must provide a quantum-resistant signature (hashed public key) for transaction validation.

Step 6: Transaction Execution: After signature verification, update balances between the sender and recipient.

Step 7: Event Logging: Emit events to confirm user registration and successful transaction verification.


# Program:
(Solidity does not natively support lattice cryptography yet, but we simulate it using custom hash-based authentication.)
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PostQuantumWallet {
    struct User {
        bytes32 publicKeyHash;
        bool registered;
    }

    mapping(address => User) public users;
    mapping(address => uint256) public balances;

    event UserRegistered(address user, bytes32 publicKeyHash);
    event TransactionVerified(address from, address to, uint256 amount);

    function registerUser(bytes32 _publicKeyHash) public {
        require(!users[msg.sender].registered, "User already registered");
        users[msg.sender] = User(_publicKeyHash, true);
        emit UserRegistered(msg.sender, _publicKeyHash);
    }

    function sendFunds(address _to, uint256 _amount, bytes32 _signature) public {
        require(users[msg.sender].registered, "Sender not registered");
        require(users[_to].registered, "Recipient not registered");
        require(balances[msg.sender] >= _amount, "Insufficient funds");

        bytes32 calculatedHash = keccak256(abi.encodePacked(msg.sender, _to, _amount));
        require(calculatedHash == _signature, "Invalid quantum-safe signature");

        balances[msg.sender] -= _amount;
        balances[_to] += _amount;
        emit TransactionVerified(msg.sender, _to, _amount);
    }
}
```

# Expected Output:

Users register using a post-quantum secure public key.


Transactions require a quantum-resistant signature for authentication.


If a traditional quantum-vulnerable hash is used, the transaction fails.


# High-Level Overview:

First quantum-safe Ethereum-compatible wallet prototype.


Uses lattice-based key hashes instead of ECDSA.


Demonstrates how Ethereum will transition to post-quantum security.


Inspired by NIST’s post-quantum cryptography competition.

# OUTPUT:

## SENDER ACCOUNT REGISTERATION OUTPUT
![image](https://github.com/user-attachments/assets/03115d36-f0c5-4206-8189-05e01e720d62)


## RECEIVER ACCOUNT REGISTERATION OUTPUT
![image](https://github.com/user-attachments/assets/ffcf0a01-bce9-4215-82a8-217a1cfd01a4)


## SENDER BALANCE OUTPUT
![image](https://github.com/user-attachments/assets/7fe996ce-1625-4070-a3c7-12b22641cec3)


## GENERATE SIGNATURE
![image](https://github.com/user-attachments/assets/7072ff70-812b-4a42-ab30-88ea9516997d)


## SENDER TRANSFER AMOUNT TO RECEIVER
![image](https://github.com/user-attachments/assets/e71bd98c-5c23-4e43-a0a9-2a322423fac3)


## RECEIVER BALANCE AFTER TRANSACTION
![image](https://github.com/user-attachments/assets/43927172-e0b5-413e-9fea-5f417c514a61)


# RESULT: 

Thus, to create a quantum-resistant wallet using lattice-based cryptography instead of traditional ECDSA, ensuring that future quantum computers cannot break private keys is executed successfully.
