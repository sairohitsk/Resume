#  Decentralized Crowdfunding Smart Contract

### Course: Blockchain Technology  
### Assignment: Programming Assignment 2  
### Deadline: October 31, 2025  
### Group No: 7  
### Members:  

| Student ID | Name |
|-------------|------|
| 2023A7PS0158H | Arpit Kudre |
| 2023A7PS0056H | Advait Gokhale |
| 2023A7PS1099H | Adityadev Rajesh |
| 2023A7PS1056H | Aditya Kumar |
| 2024H1030158H | Sai Rohit Shaik |

---

##  Overview
This Solidity smart contract implements a **trustless crowdfunding platform** designed for an independent artist who wants to raise funds transparently. It ensures that:
- Funds are released to the project creator **only** when the funding goal is achieved before the campaign deadline.
- If the goal is **not met**, all contributors can reclaim their exact contributions.
- No party can withdraw funds before the campaign concludes.

The contract eliminates intermediaries and ensures transparent handling of funds on the blockchain.

---

##  Contract Summary

**Contract Name:** `CrowdFunding`  
**Language:** Solidity (v0.8.20 or later)

### Key Variables
| Variable | Type | Description |
|-----------|------|-------------|
| `creator` | `address payable` | Project creator’s wallet address (contract deployer). |
| `fundingGoal` | `uint` | Target amount (in wei) for the campaign. |
| `deadline` | `uint` | UNIX timestamp marking campaign end. |
| `totalFunds` | `uint` | Total Ether received from contributors. |
| `contributors` | `mapping(address => uint)` | Stores how much each backer contributed. |
| `goalReached` | `bool` | True if the target amount is achieved. |
| `campaignEnded` | `bool` | Indicates campaign completion. |

---

##  Function Details

### `constructor(uint _goal, uint _durationInMinutes)`
Initializes campaign settings.  
- Deployer becomes the creator.  
- Sets the deadline as current time + duration.

### `contribute()`
Allows supporters to send Ether.  
- Only before the deadline.  
- Tracks contribution per address.  
- Emits `ContributionReceived`.

### `checkGoalReached()`
Evaluates whether the campaign succeeded or ended.  
- Marks `goalReached = true` if target met.  
- Sets campaign as ended after deadline.

### `withdrawFunds()`
Lets the **creator** claim funds **only** after campaign success.  
- Restricts early withdrawals.  
- Emits `FundsWithdrawn`.

### `refund()`
Lets each contributor get their Ether back if the goal wasn’t reached.  
- Each backer can claim once.  
- Uses zeroing to prevent reentrancy.  
- Emits `RefundIssued`.

### Events
| Event | Description |
|--------|--------------|
| `ContributionReceived(address, uint)` | Triggered on contribution. |
| `FundsWithdrawn(address, uint)` | Triggered on successful withdrawal. |
| `RefundIssued(address, uint)` | Triggered on refund. |

---

##  Campaign Lifecycle

###  Successful Campaign
1. Creator deploys with a funding goal and time limit.  
2. Contributors fund the campaign.  
3. When funds ≥ goal → creator withdraws after deadline.  

**Flow:**  
`Deploy → Contribute → Goal Reached → Deadline Passed → Creator Withdraws`

###  Failed Campaign
If the goal is not met before the deadline:  
- Each backer reclaims their amount via `refund()`.

**Flow:**  
`Deploy → Contribute → Deadline Passed → Refund`

---

##  Remix IDE Testing
1. Open [Remix IDE](https://remix.ethereum.org).  
2. Paste the contents of `CrowdFunding.sol`.  
3. Compile using **Solidity 0.8.20 or later**.  
4. Deploy using **Remix VM (London)**.  
5. Provide constructor inputs:  
   - `_goal` (e.g., `5 ether`)  
   - `_durationInMinutes` (e.g., `2`)  
6. Test both success and failure paths by contributing from multiple accounts.

---

##  Security
- Reentrancy protection (`refund()` resets before transfer).  
- Restricted withdrawals (creator-only).  
- Deadline and goal validation using `require()`.  
- Prevents duplicate refunds.

---

##  Viva Preparation
Be ready to explain:
- Use of `msg.sender`, `msg.value`, and `payable`.  
- Solidity function visibility (`public`, `external`, etc.).  
- Events and their role in transparency.  
- Reentrancy prevention strategy.

---

##  Conclusion
The contract meets all assignment requirements — enabling secure, decentralized crowdfunding with proper handling of contributions, refunds, and transparent fund flow.

---
