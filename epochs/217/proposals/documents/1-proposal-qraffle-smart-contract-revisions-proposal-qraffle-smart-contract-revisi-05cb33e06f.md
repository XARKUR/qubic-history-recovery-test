# Proposal-Qraffle-Smart-Contract-Revisions# Proposal: Qraffle Smart Contract Revisions

Revert specific overly restrictive changes introduced in the previous Qraffle smart contract upgrade to restore broader accessibility and functionality for token raffles.

## Available Options

- **Option 0:** No, do not approve the Qraffle revisions.
- **Option 1:** Yes, approve the Qraffle revisions (revert the listed restrictions).

## Context

The previous upgrade to the Qraffle smart contract introduced several improvements for security, performance, and bug fixes. However, certain changes have proven too restrictive in scope and functionality for token raffles, limiting community participation and practical use of many tokens in the Qubic ecosystem. This proposal targets only those specific restrictions for reversion, while preserving all other enhancements from the prior upgrade.

## Summary of Changes (Reversions)

### Accessibility & Participation

- Remove the new requirement that **only registered DAO members** can participate in token raffles. Revert to the original state where **anyone** can participate.
- This restores open participation and prevents unnecessary barriers for the broader Qubic community.

### Token Eligibility

- Remove the exclusion of **QRAFFLE shares** and **QXMR** tokens from being used in raffles.
- Revert to the original state where **no tokens are excluded** — all tokens in the ecosystem can be used as raffle prizes (as intended).

### Proposal Thresholds

- Lower/remove the newly added **1,000,000 token minimum** for new token raffle proposals.
- This threshold is too high and prevents practical use of many smaller or emerging tokens in the ecosystem. Reverting allows more flexible and realistic raffle proposals.

## Rationale

These restrictions, while well-intentioned for security and spam prevention in the prior upgrade, have inadvertently reduced the utility and inclusivity of the Qraffle platform. Reverting them brings token raffles back to their original, more open design while maintaining the security, fee accounting, performance, and randomness improvements from the last update.

## Technical Implementation

**Developer:** https://github.com/double-k-3033

Core implementation PR: https://github.com/qubic/core/pull/898
