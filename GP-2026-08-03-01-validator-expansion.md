# Governance Proposal: Expansion of the Enjin Relaychain Active Validator Set from 25 to 35

Date: 2026-08-03

Proposal ID: GP-2026-08-03-01

Submitted by: Enjin

## Summary

This proposal initiates a staged expansion of the Enjin Relaychain active validator set from 25 to 35 validators over twelve months. This referendum enacts the first stage, an increase of the active validator count from 25 to 28, and sets out the full expansion schedule. Each subsequent stage will be enacted by its own referendum on the Staking Admin track. The expansion is the first phase toward a long-term goal of 50 active validators.

## Motions

Motion 1

Increase the active validator count of the Enjin Relaychain from 25 to 28 by executing `staking.increaseValidatorCount(3)` via the Staking Admin origin, effective at the first era election following enactment.

Motion 2

Endorse the following twelve-month expansion schedule, with each stage subject to its own referendum on the Staking Admin track and contingent on continued network health:

| Stage | Referendum submission | In force (target) | Change | Active set |
|---|---|---|---|---|
| 1 (this referendum) | Mon 3 Aug 2026 | Tue 18 Aug 2026 | +3 | 28 |
| 2 | Mon 2 Nov 2026 | Tue 17 Nov 2026 | +3 | 31 |
| 3 | Mon 1 Feb 2027 | Tue 16 Feb 2027 | +2 | 33 |
| 4 | Mon 3 May 2027 | Tue 18 May 2027 | +2 | 35 |

New validators enter at the first era election after each stage takes force, typically within one to two eras (days).

## Rationale

1. **Decentralization and resilience.** The set of 25 active validators is a deliberately conservative configuration dating to network launch in 2023. The network has since matured through more than a thousand consecutive eras of block production. Widening the set reduces reliance on any single operator, improves geographic and provider diversity, and strengthens the network's resilience.

2. **Demonstrated demand.** 27 validators are currently registered for 25 slots, and additional reputable operators have approached Enjin seeking capacity. Demand for slots exceeds supply today.

3. **Staged and adjustable.** Enacting one stage per quarter, each by its own referendum, lets the network verify performance between stages. The schedule can be paused or revised through governance at any point, with no special cancellation mechanism required.

4. **Quality of new entrants.** Slots opened by an increased validator count are filled permissionlessly by the era election. Enjin will monitor each stage of the expansion and work with validator operators as needed to maintain network stability and quality of operation.

## Validator Economics

Live network values (31 July 2026, era 1137): total issuance ~2.010 billion ENJ; annual inflation 4.8927%, of which 96% flows to relaychain stakers; ~449.6 million ENJ staked (~22.4% of supply); 258,641 ENJ distributed to stakers in the most recent era (one era = one day). Era rewards are split approximately evenly across active validators; the standard validator commission across the current set is 5%.

Per-validator income at 5% commission as the set expands:

| Active set | Reward / validator / day | Commission / month | Change vs. today |
|---|---|---|---|
| 25 (today) | ~10,350 ENJ | ~15,700 ENJ | — |
| 28 | ~9,240 ENJ | ~14,000 ENJ | −10.7% |
| 31 | ~8,340 ENJ | ~12,700 ENJ | −19.4% |
| 33 | ~7,840 ENJ | ~11,900 ENJ | −24.2% |
| 35 | ~7,390 ENJ | ~11,200 ENJ | −28.6% |

Per-validator commission income declines roughly 29% across the full expansion. This is the transparent trade-off of decentralization. Nominator and pool-member yield is unaffected by the validator count itself: the same reward pool is distributed over the same staked supply. Average stake backing per validator declines from ~18.0M to ~12.8M ENJ as existing nominations spread across a larger set.

No supplemental incentives, treasury spends, or token issuance changes are attached to this proposal. Validators are expected to sustain operations from standard network rewards. Progression beyond 35, toward the long-term 50-validator goal, will be proposed separately and remains conditional on validator economics staying viable at each scale.

## Risks and Considerations

- **Election is permissionless.** An increased count admits whichever candidates win the era election. The staged schedule allows the network to assess the quality and performance of each intake before the next stage is proposed.
- **Backing dilution.** Lower average backing per validator is an expected and accepted consequence; total economic security (total stake) is unchanged.
- **Validator revenue decline.** Fiat-denominated validator economics depend on ENJ price. Because each stage requires its own referendum, the schedule adjusts through governance if operating a validator becomes uneconomical.
- **No runtime changes.** This proposal executes a single existing staking-pallet call. No code changes, no migrations, no new pallets.

## On-Chain Execution

| Field | Value |
|---|---|
| Network | Enjin Relaychain |
| Runtime at drafting | `enjin/1070` |
| Call | `staking.increaseValidatorCount(additional: 3)` |
| Encoded call data | `0x090a0c` |
| Preimage hash (blake2-256) | `0xc6bcbdbd258dc3b9632affdd2d7bf444dfae386ce6b734c58c0a3604b7219cb9` |
| Preimage length | 3 bytes |
| Track | `staking_admin` (ID 100) |
| Dispatch origin | `Origins.StakingAdmin` |
| Decision deposit | 100,000 ENJ (refundable on decision) |
| Track periods | prepare 2 h · decide up to 14 days · confirm 3 h |
| Enactment | `At(block ≈ 16,864,000)`, ≈ Tuesday 18 August 2026 12:00 UTC |
| Effect | `Staking::ValidatorCount` storage: 25 → 28, applied at the next era election |

To verify the on-chain referendum matches this text, compare the preimage hash above with the one shown on [gov.enjin.cloud](https://gov.enjin.cloud) or Polkadot-JS Apps. The runtime authorizes the Staking Admin origin to execute this call.

## Voting and Enactment

This referendum is submitted on the Staking Admin track on Monday 3 August 2026, with a decision period of up to 14 days. Enactment is fixed at a specific block (approximately 16,864,000), corresponding to Tuesday 18 August 2026 around 12:00 UTC, so the change takes force on a known date regardless of when during the decision period the referendum confirms. The active set expands at the first era election after enactment, expected 19–20 August 2026.
