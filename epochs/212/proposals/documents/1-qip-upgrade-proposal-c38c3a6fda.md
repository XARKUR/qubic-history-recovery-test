# QIP Upgrade Proposal 

**Proposer:** NINEISTEN

**Date:** April 30, 2026

**Epoch:** 212

## Summary
This proposal requests Quorum approval to integrate the QIP (Qubic ICO Portal) upgrades from the following commit:

**Commit:** https://github.com/baoLuck/core/commit/2193a89e8a4140bbc0f08d5578ed84f3ae53e096  
**Files changed:** Primarily `QIP.h` (core contract logic)

## Problem / Opportunity
The current QIP contract lacks modern features for ICOs, investor protection, fee distribution, and capital efficiency. The proposed changes introduce vesting mechanics, fund return capabilities, token burning for unsold portions, and improved fee handling — directly benefiting investors, creators, and the broader Qubic ecosystem.

## Proposed Solution
Approve and integrate the exact changes from the referenced commit into the active QIP smart contract / core protocol.

**Key Upgrades Included (from diff):**
- **New Constants & Fee Logic**:
  - ICO setup fee (1b QU)
    - 90% is split between Dev fund (33%) & shareholder (66%)
    - 10% feeds the SC reserve
  - 97% of ICO revenue goes to the Token's Project team, 1% goes towards the QIP development fund, & shareholders recieve the final 2%.
  - max Project development access Tranche is 40% when vesting is selected. This will be immediately distributed to the token's project team to fund their development.
- **New Data Structures**: `BuyerInfo` tracking, `IcoBuyerKey` for buyer management (HashMap size 131072).
- **Vesting & Burning Features**: `burnRemainingTokens`, `isVested` + `vestingPeriod`, extended `ICOInfo`.
- **New Procedures**: `returnFunds_input/output` for safe fund returns.
- **Improved createICO & buyToken Logic**: Stricter validation, proper fee refunds, excess reward handling, and post-fee share transfers.
- **Enhanced getICOInfo** output with new fields.

## Benefits
- Stronger investor protections via vesting and fund return mechanisms.
- Better capital efficiency (burn unsold tokens, optimized fee distribution).
- Reduced risk of failed ICOs and improved creator tooling.
- Maintains full backward compatibility while enabling advanced ICO features.
- Aligns with Qubic’s decentralized governance and smart contract standards.

## Risks & Mitigations
- **Low risk**: Changes are non-breaking, well-structured, and include extensive validation.
- **Tested in repo**: Includes updates to test files (`contract_qip.cpp`).
- No impact on existing deployed ICOs.

## Voting Options
- **Option 1**: **Approve the QIP upgrades**
- **Option 0**: Reject the upgrades 

## Next Steps if Approved
1. Merge the updated QIP contract into the Qubic core.
2. Proceed with integration and development at QubicICO.com
3. Update documentation and notify the community.

**References**  
- Commit: https://github.com/baoLuck/core/commit/2193a89e8a4140bbc0f08d5578ed84f3ae53e096  
