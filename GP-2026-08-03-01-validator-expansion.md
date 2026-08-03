# Governance Proposal: Expansion of the Enjin Relaychain Active Validator Set from 25 to 35

Date: 2026-08-03

Proposal ID: GP-2026-08-03-01

Submitted by: Enjin

## Summary

This proposal expands the Enjin Relaychain active validator set from 25 to 35 validators over twelve months, in four stages enacted by this single referendum: the first increase applies immediately at enactment, and the remaining three are pre-scheduled on-chain at 90-day intervals. This initial expansion advances the secure, decentralized network envisioned in the Enjin Blockchain whitepaper. Once the twelve-month expansion completes, it will be reviewed with a view to continuing from 35 to 50; beyond that, the size of the validator set will be determined by the network through its own governance.

## Motions

Motion 1

Increase the active validator count of the Enjin Relaychain from 25 to 27 by executing `staking.increaseValidatorCount(2)` via the Root origin at enactment, effective at the first era election following it.

Motion 2

Pre-schedule the remaining three increases on-chain as named scheduler tasks, executing automatically at 90-day intervals from enactment:

| Stage | Executes at block | Approx. date | Change | Active set |
|---|---|---|---|---|
| 1 (at enactment) | 16,950,720 | ≈ 24 Aug 2026 | +2 | 27 |
| 2 (`nodesmith/t2`) | 18,246,720 | ≈ 22 Nov 2026 | +3 | 30 |
| 3 (`nodesmith/t3`) | 19,542,720 | ≈ 20 Feb 2027 | +2 | 32 |
| 4 (`nodesmith/t4`) | 20,838,720 | ≈ 21 May 2027 | +3 | 35 |

New validators enter at the first era election after each stage executes, typically within one to two eras (days). Scheduled dates are approximate, as block production drifts slightly against wall-clock time.

Should network conditions warrant a pause, any scheduled stage can be cancelled before it executes by a subsequent Root-track referendum calling `scheduler.cancelNamed` with the task id listed above.

## Rationale

1. **Decentralization and resilience.** The set of 25 active validators is a deliberately conservative configuration dating to network launch in 2023. The network has since matured through more than a thousand consecutive eras of block production. Widening the set reduces reliance on any single operator, improves geographic and provider diversity, and strengthens the network's resilience.

2. **Demonstrated demand.** 27 validators are currently registered for 25 slots, and additional reputable operators have approached Enjin seeking capacity. Demand for slots exceeds supply today.

3. **Staged, transparent, and reversible.** The full twelve-month schedule is enshrined on-chain by this referendum and visible in the scheduler agenda, giving operators and nominators certainty about when capacity opens. The 90-day spacing gives the network a full quarter at each size, and any pending stage can be cancelled by a further referendum if conditions change.

4. **Quality of new entrants.** Slots opened by an increased validator count are filled permissionlessly by the era election. Enjin will monitor each stage of the expansion and work with validator operators as needed to maintain network stability and quality of operation.

## Validator Economics

Live network values (31 July 2026, era 1137): total issuance ~2.010 billion ENJ; annual inflation 4.8927%, of which 96% flows to relaychain stakers; ~449.6 million ENJ staked (~22.4% of supply); 258,641 ENJ distributed to stakers in the most recent era (one era = one day). Era rewards are split approximately evenly across active validators; the standard validator commission across the current set is 5%.

Per-validator income at 5% commission as the set expands:

| Active set | Reward / validator / day | Commission / month | Change vs. today |
|---|---|---|---|
| 25 (today) | ~10,350 ENJ | ~15,700 ENJ | — |
| 27 | ~9,580 ENJ | ~14,600 ENJ | −7.4% |
| 30 | ~8,620 ENJ | ~13,100 ENJ | −16.7% |
| 32 | ~8,080 ENJ | ~12,300 ENJ | −21.9% |
| 35 | ~7,390 ENJ | ~11,200 ENJ | −28.6% |

*Figures are computed from the live network values above (era 1137). The absolute ENJ amounts will drift as total issuance and the staked supply change; the percentage changes depend only on the size of the set.*

Per-validator commission income declines roughly 29% across the full expansion. This is the transparent trade-off of decentralization. Aggregate staker rewards are unaffected by the validator count itself: the same reward pool is distributed over the same staked supply. Individual nominator and pool returns depend, as they do today, on the performance and backing of the validators their stake supports; a wider set spreads existing stake across more validators. Average stake backing per validator declines from ~18.0M to ~12.8M ENJ as existing nominations spread across a larger set.

No supplemental incentives, treasury spends, or token issuance changes are attached to this proposal. Validators are expected to sustain operations from standard network rewards. Progression from 35 to 50 would follow a review of this expansion and come by separate proposal, conditional on network health and validator economics staying viable at each scale.

## Risks and Considerations

- **Election is permissionless.** An increased count admits whichever candidates win the era election. The staged schedule allows the network to assess the quality and performance of each intake before the next stage executes.
- **Scheduled stages execute automatically.** Pausing a pending stage requires a Root-track referendum cancelling its named task before its scheduled block. The 90-day spacing leaves ample time: a cancellation referendum completes in at most ~15 days.
- **Backing dilution.** Lower average backing per validator is an expected and accepted consequence; total economic security (total stake) is unchanged.
- **Validator revenue decline.** Fiat-denominated validator economics depend on ENJ price. The review before any continuation beyond 35 accounts for this.
- **No runtime changes.** This proposal executes existing staking, scheduler, and utility pallet calls. No code changes, no migrations, no new pallets.

## On-Chain Execution

| Field | Value |
|---|---|
| Network | Enjin Relaychain |
| Runtime at drafting | `enjin/1070` |
| Call | `utility.batchAll([staking.increaseValidatorCount(2), scheduler.scheduleNamed(nodesmith/t2, 18246720, increaseValidatorCount(3)), scheduler.scheduleNamed(nodesmith/t3, 19542720, increaseValidatorCount(2)), scheduler.scheduleNamed(nodesmith/t4, 20838720, increaseValidatorCount(3))])` |
| Preimage hash (blake2-256) | `0x251c5d288190a8609609bfc1dcb9bf6b2b517b6a6b4e13bb12ef4475547d7c27` |
| Preimage length | 135 bytes |
| Track | `root` (ID 0) |
| Dispatch origin | Root |
| Decision deposit | 1,000,000 ENJ (refundable on decision) |
| Track periods | prepare 2 h · decide up to 14 days · confirm 24 h |
| Enactment | `At(block 16,950,720)`, ≈ Monday 24 August 2026 |
| Scheduler task ids | `nodesmith/t2` = `0x6e6f6465736d6974682f74320000…0000` · `t3`, `t4` analogous (ASCII, zero-padded to 32 bytes) |
| Effect | `Staking::ValidatorCount`: 25 → 27 at enactment, then 30 / 32 / 35 automatically at the scheduled blocks |

Full encoded call data:

```
0x140210090a0864026e6f6465736d6974682f74320000000000000000000000000000000000000000
406c1601003f090a0c64026e6f6465736d6974682f74330000000000000000000000000000000000
000000c0322a01003f090a0864026e6f6465736d6974682f74340000000000000000000000000000
00000000000040f93d01003f090a0c
```

To verify the on-chain referendum matches this text, compare the preimage hash above with the one shown on [gov.enjin.cloud](https://gov.enjin.cloud) or Polkadot-JS Apps. The call requires the Root origin, which the Root track provides; the scheduled tasks dispatch with the same origin.

## Voting and Enactment

This referendum is submitted on the Root track on Monday 3 August 2026, with a decision period of up to 14 days. Enactment is fixed at block 16,950,720, approximately Monday 24 August 2026, when the first increase applies; the active set expands at the following era election. The remaining increases execute automatically at the scheduled blocks, approximately 22 November 2026, 20 February 2027, and 21 May 2027.
