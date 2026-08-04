# Proposal-Optimize-QBOND-Smart-Contract

Optimize the existing Qbond Smart Contract on Qubic.

## Available Options

- **Option 0:** No, do not approve the Qbond optimize.
- **Option 1:** Yes, approve the Qbond optimize.

## Context

Qbond already exists on-chain. This proposal is an optimize proposal, not an initial deployment proposal.

## Summary of Changes

### What we optimized?

- To reduce spamming of procedures, discourage micro-transactions, and improve the economic model of the contract, we introduced additional fees and stricter minimum thresholds for staking, bidding, and burning.

### Staking Adjustments (`Stake`, procedure 1)

- **Minimum Stake Amount per Call:** `constexpr uint64 QBOND_MIN_MBONDS_TO_STAKE = 10ULL;`. Users must stake a minimum of 10 MBND per `Stake` invocation.
- **Current behavior:** A user can stake as little as 1 MBND. The amount is placed into `_stakeQueue` and waits until the queue accumulates at least 10 MBND before the actual staking is executed.
- **New behavior:** Any `Stake` call with `input.quMillions < 10` is rejected immediately. The queue threshold remains for epoch stake-limit handling, but micro-stakes of 1 MBND are no longer accepted.
- **Stake Fee:** `QBOND_STAKE_FEE_PERCENT` is reduced from `50` (0.5%) to `40` (0.4%).
- **Fee savings:** The 0.1 percentage-point reduction (from 0.5% to 0.4%) means users save **0.1%** on every stake. For every **100 million QUs** staked, this equals **100,000 QUs** saved in commission fees — the same amount as one `AddAskOrder` prepayment (`QBOND_ASK_ORDER_FEE`).

### Order Book — Ask Side

- **`AddAskOrder` (procedure 3):** `constexpr uint64 QBOND_ASK_ORDER_FEE = 100000ULL;`. The invocator must pay 100,000 QUs when placing an ask order. This amount acts as a **prepayment toward the 0.03% trade fee** (`QBOND_TRADE_FEE_PERCENT`) charged when the order is matched:
  - On match, the trade fee is calculated as usual: `tradeFee = matchedValue * 0.03%`.
  - If `tradeFee <= 100,000`: no additional fee is deducted from the trade proceeds — the prepayment fully covers the trade fee.
  - If `tradeFee > 100,000`: only the excess (`tradeFee - 100,000`) is deducted from the trade proceeds on top of the prepayment already paid.
  - In effect, 100,000 QUs is the minimum trade-fee prepayment per ask order; the seller never pays more than the standard 0.03% trade fee in total.
- **`RemoveAskOrder` (procedure 4):** No procedure fee. Partial ask-order removal remains supported.

### Order Book — Bid Side

- **`AddBidOrder` (procedure 5):** `constexpr uint64 QBOND_MIN_BID_AMOUNT = 1000000ULL;`. The total bid value (`numberOfMBonds * price`) must be at least 1,000,000 QUs. No additional procedure fee.
- **`RemoveBidOrder` (procedure 6):** The invocator must pay `QBOND_PROCEDURE_FEE` (100,000 QUs) on every `RemoveBidOrder` invocation — both full and partial removal. The fee is burned via `qpi.burn()`. Partial bid-order removal remains supported, but after a partial removal the remaining bid value (`remainingMBonds * price`) must still be at least `QBOND_MIN_BID_AMOUNT` (1,000,000 QUs). Calls that would leave less than 1M in the order are rejected.
- **Automatic bid refund on reward payout:** When staking rewards are distributed at the end of the full cycle (52 epochs of staking + reward epoch in `BEGIN_EPOCH`), all remaining bid orders for that MBND epoch are refunded. Each refund amount is reduced by 100,000 QUs; the deducted amount is burned.

### Burn and Admin

- **`BurnQU` (procedure 7):** `constexpr uint64 QBOND_MIN_BURN_AMOUNT = 5000000ULL;`. Users must burn a minimum of 5,000,000 QUs per `BurnQU` invocation.
- **`UpdateCFA` (procedure 8):** No changes.

### Fee Policy

- Procedure fees (`RemoveBidOrder`, automatic bid refunds in `BEGIN_EPOCH`) are burned via `qpi.burn()` and are not added to `_totalEarnedAmount`.
- The `AddAskOrder` prepayment and any excess trade fee on match follow the existing trade-fee flow (`_totalEarnedAmount` / `_earnedAmountFromTrade`), consistent with the current 0.03% trade commission model.
- The reduced stake fee (0.4%) continues to be collected into `_totalEarnedAmount` as today.

## New Constants

| Constant | Value | Used in |
|----------|-------|---------|
| `QBOND_STAKE_FEE_PERCENT` | `40` (was `50`) | `Stake` — 0.4% stake fee (was 0.5%) |
| `QBOND_MIN_MBONDS_TO_STAKE` | `10` | `Stake` — minimum MBND per invocation |
| `QBOND_ASK_ORDER_FEE` | `100000` | `AddAskOrder` — trade-fee prepayment; credited against 0.03% fee on match |
| `QBOND_PROCEDURE_FEE` | `100000` | `RemoveBidOrder`, `BEGIN_EPOCH` bid refunds |
| `QBOND_MIN_BID_AMOUNT` | `1000000` | `AddBidOrder` — minimum total bid value; `RemoveBidOrder` — minimum remaining value after partial removal |
| `QBOND_MIN_BURN_AMOUNT` | `5000000` | `BurnQU` — minimum burn amount |

## Technical Implementation

https://github.com/qubic/core/pull/914

