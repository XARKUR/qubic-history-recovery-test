# QXMR Staking Smart Contract Proposal (Revised)

## Introduction

QXMR Staking helps Qubic by burning tokens weekly and reducing supply. This smart contract takes mining rewards and distributes them to stakers while removing Qubic from circulation. QXMR holders can stake their tokens and get mining rewards, and we believe these rewards should benefit not just miners and stakers, but also charitable causes that make a real difference.

To be built by the Qearn team, Serendipity and Poly.
Funding Request: $18,000 USD (6B Qu @ 0.000003)
Wallet: GBBDPCKSUDPTAFOIROJCFPRLEPXBJSTMKFONPDVBHBWTPUDVFEKRYPSBLLIA

Key Features

- **Deflationary:** 5% of Qubic rewards is burned, reducing overall Qubic supply.
- **Shareholder Rewards:** 5% of rewards allocated to smart contract shareholders.
- **Social Impact:** 5% donated to real-world causes, positioning Qubic as a community-minded ecosystem.
- **Reward Distribution:** Qubic payouts calculated based on stake size and duration, ensuring fair and transparent rewards.
- **QXMR Burning:** Optional QXMR burn feature to boost APY, promoting token scarcity and enhancing value for all holders.
- **Dashboard:** Real-time analytics displaying stake performance, QXMR earnings, and lock history for informed decision-making.

## Business Model

Qubic's mining technology is cutting edge, but mining is getting harder. Because of the XMR mining proof of concept, more people are mining Qubic, which increases difficulty. Now you need newer, expensive CPUs to make money from mining - many people can't afford this hardware. This creates a bigger problem - lots of Qubic supporters feel left out because they can't mine profitably with older computers.

QXMR Staking fixes this by letting anyone earn mining rewards through staking, no matter what computer they have. This keeps people engaged in the frontlines of Qubic mining, the heartbeat of our ecosystem.

QXMR token holders can stake their tokens in a smart contract seamlessly integrated with Qubic’s mining operations, earning rewards in Qubic proportional to their stake size and chosen duration.

An optional burn mechanism allows stakers to sacrifice a portion of their QXMR to boost their APY, enhancing returns while increasing QXMR scarcity to benefit all holders.

QXMR mining operations currenty range from 25 to 30 million Qubic. These mining rewards will be directed to a QXMR Staking wallet and distributed as follows:
- 50% to stakers
- 35% to fund mining operations
- 5% Qubic burned
- 5% to shareholders
- 5% to charity (Kimz300)

## Components to Be Developed (High-Level)

### Frontend UI:
- Similar with Qearn UI.
- Simple Lock/Unlock Form
- Burn-to-Boost Form
- Lock History table
- Analytics page of QXMR staking
- WalletConnect
- Hosted at [https://stake.qxmr.quest/](https://stake.qxmr.quest/)

### Smart Contract:
- Staking for QXMR deposits
- Reward distribution for Qubic payouts
- Optional Burn-to-Boost logic
- Weekly Qubic burning
- Weekly charitable donation

## Technical Architecture (High-Level)
- **Smart Contract:** C++
- **Frontend:** React + Vite + Qubic Connect

## User Journey/Examples

User:
- Visits [https://stake.qxmr.quest/](https://stake.qxmr.quest/)
- Connects Qubic wallet with WalletConnect
- Chooses amount of QXMR to stake
- Selects staking period:
  - 1 Month
  - 3 Months
  - 6 Months + 0.4% QXMR Bonus
  - 12 Months + 1% QXMR Bonus
- Selects optional Burn-to-Boost APY:
  - Bronze - Burn 5% QXMR ⇒ Boost APY +10%
  - Silver - Burn 10% QXMR ⇒ Boost APY +25%
  - Gold - Burn 20% QXMR ⇒ Boost APY +50%
  - Platinum - Burn 30% QXMR ⇒ Boost APY +80%
- Stakes QXMR

***Example UI and APY (UI mockup - not representative of Serendipity's work. UI will resemble Qearn's***

![QXMR Staking Options](staking%20qxmr.png)
![QXMR Rewards Boost](rewards%20boost.png)

## Detailed Scope and Timeline

### Timeline (6-8 weeks):
- **Week 1:** Design UI mockups and smart contract architecture
- **Week 2-4:** Prototype with staking and reward functionality
- **Week 5-6:** Smart contract integration and final testing
- **Week 7-8:** Audit, IPO, and Mainnet deployment

## Milestones and Deliverables

**Milestone 1 (1 Week):**
- Create user interface mockup without logic implementation
- Smart contract architecture docs

**Milestone 2 (3 Weeks):**
- Wallet connect
- Qubic core services integration
- Develop a functional prototype implementing staking and reward distribution features
- Smart contract first version

**Milestone 3 (2 Weeks):**
- Integration of UI with smart contract
- Final smart contract with possible updates for providing better data and interaction with UI
- Smart contract test script

**Milestone 4 (2 Weeks):**
- Audit, IPO, and Mainnet deployment

## Payment Terms

**Funding Request:** $18,000 USD
- Breakdown: 100% for devs

**Disbursement**
- 20% M1: UI mockup (without functionalities), smart contract docs
- 30% M2: Alpha version of UI and smart contract
- 20% M3: All test finished beta version
- 30% M4: Mainnet deployment

## Team Composition

- **Profitphil:** Team Lead
- **Serendıpıtч:** Lead frontend UI developer
- **Poly:** Lead smart contract developer

-Website: https://www.qxmr.quest
-X: @_qxmr_token_
