# Hello Ethernaut

## Objective

Interact with the deployed contract to uncover the password and call the `authenticate()` function with the correct value to complete the level.

---

## Getting Started

1. Go to [ethernaut.openzeppelin.com](https://ethernaut.openzeppelin.com/)
2. Select the **"Hello Ethernaut"** level
3. Click **“Get new instance”** and approve the transaction in MetaMask
4. The instance is now deployed and available as the `contract` object in the browser console
5. Save the instance address:

    ```js
    contract.address
    ```

---

## Walkthrough

### Step 1: Follow the Contract's Hint Chain

```js
await contract.info()
await contract.info1()
await contract.info2("hello")
await contract.infoNum()
await contract.info42()
await contract.theMethodName()
await contract.method7123949()
```

**Explanation:**
Each function provides a hint or directs you to the next function. This sequence leads you to the point where the contract asks for a password.

---

### Step 2: Retrieve the Password

```js
await contract.password()
```

**Explanation:**
The password is stored in a public variable. Reading it directly reveals the value needed for authentication.

---

### Step 3: Authenticate Using the Password

```js
await contract.authenticate("ethernaut0")
```

**Explanation:**
Calling `authenticate()` with the correct password completes the challenge. Confirm the transaction in MetaMask.

---

## What You Learn

This level teaches the following:

* How to interact with smart contracts using JavaScript in the browser console
* Reading public state variables and functions
* Navigating contract hints and function chains
* Sending transactions and authenticating using MetaMask

---

## Resources Used

* [Ethernaut Game](https://ethernaut.openzeppelin.com/)
* [Solidity Docs](https://docs.soliditylang.org/)
* [Video Walkthrough](https://www.youtube.com/watch?v=Hzu36mTJLeA)
* [Blog Writeup](https://shubhamm.me/blog/07-ethernaut-challenges-hello-ethernaut)

---
