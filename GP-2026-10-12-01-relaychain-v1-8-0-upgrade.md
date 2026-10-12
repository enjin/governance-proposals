# Governance Proposal: Enjin Relaychain Runtime Upgrade to v1.8.0 ("Fort Canning")

Date: 2026-10-12

Proposal ID: GP-2026-10-12-01

Submitted by: Enjin

## Summary

This proposal authorizes the upgrade of the Enjin Relaychain runtime from v1.7.0 (spec version 1070) to v1.8.0 (spec version 1080), codenamed Fort Canning. The referendum authorizes the new runtime's code hash at block 18,059,050, about five minutes ahead, and the authorized code is then applied in block 18,059,100, approximately Monday 9 November 2026 at 16:00 UTC. The upgrade tunes the governance parameters set at launch, adds dispute slashing, brings the token and marketplace features shared with the Matrixchain, and updates the Polkadot SDK to stable2606. The Matrixchain upgrade follows one week later under its own proposal, GP-2026-11-10-01.

> **Action required.** Validators must run client v1.8.0; other nodes are recommended to also upgrade. Wallets, dependencies and exchanges that hard-code the transaction version must switch from 18 to 19 at enactment. Details are in the [changelog](https://enjin.io/changelog/fort-canning).

## Motion

Authorize runtime code hash `0xb3c5e0dbd0e13b5936d8aead6cbcb505ddb71a671094190a7f08c7e4b2a5a0da` (blake2-256 of `enjin_runtime.compact.compressed.wasm`, the mainnet runtime of Relaychain release v1.8.0, 2,122,534 bytes) by executing `system.authorizeUpgrade(0xb3c5e0dbd0e13b5936d8aead6cbcb505ddb71a671094190a7f08c7e4b2a5a0da)` with the Root origin, with enactment fixed at block 18,059,050. `checkVersion` is true, so the code is verified to carry `spec_name` `enjin` and a higher `spec_version` before it can be applied. Enjin then applies the authorized code in block 18,059,100 with `system.applyAuthorizedUpgrade(code)`, and v1.8.0 runs from the following block.

## Changes

- **Governance parameters.** The submission deposit rises from 0.025 ENJ to 1,000 ENJ on every track. Decision deposits rise on 18 of 19 tracks: root and fellowship admin to 2,500,000 ENJ, the other admin tracks to 1,000,000 ENJ, and the treasury tracks to a stepped share of their spend cap. Every track gets a support floor (0.5% on root and the admin tracks, 0.005% to 0.125% on the treasury tracks), the whitelisted-caller floor falls from 1% to 0.25%, concurrent referenda per treasury track are cut, and support is measured against total issuance so ENJ on the Matrixchain counts. Periods, curve shapes and origins are unchanged. The launch values are tuned so that deposits and support requirements match their purpose: fair and decentralized governance, with referenda decided by a meaningful share of ENJ holders. The new values apply to referenda still deciding at enactment, and the Matrixchain's new OpenGov tracks use the same values.
- **Validator accountability.** The standard dispute-slashing handler applies, as on Polkadot and Kusama: 100% of stake for backing an invalid candidate and 2% for approving one, deferred 27 eras and cancellable by governance, with the validator disabled for the rest of the era. The GRANDPA authority limit rises from 32 to 100, which the validator-set expansion decided in GP-2026-08-03-01 needs.
- **Shared token and marketplace features.** The marketplace upgrade, token lending, ephemeral tokens, mint rate limits and attribute freezing ship in the shared pallets, keeping the Relaychain's multi-tokens and marketplace (sENJ, Degens) at parity with the Matrixchain.
- **Staking and marketplace.** Nomination pool rewards are paid per exposure page, so payouts keep working as pools grow, and the stake-exchange RPC is served again. Auction bids in ENJ are held in escrow from placement, and abandoned settlements can be resolved permissionlessly.
- **Polkadot SDK stable2606**.
- Bug fixes and security improvements.

## On-Chain Execution

| Field | Value |
|---|---|
| Network | Enjin Relaychain |
| Runtime at drafting | `enjin/1070`, transaction version 18 |
| Call | `system.authorizeUpgrade(0xb3c5e0dbd0e13b5936d8aead6cbcb505ddb71a671094190a7f08c7e4b2a5a0da)` (pallet 0, call 9) |
| Encoded call data | `0x0009b3c5e0dbd0e13b5936d8aead6cbcb505ddb71a671094190a7f08c7e4b2a5a0da` |
| Proposal hash (blake2-256) | `0xaa04cc2c31d7cf2374399be8995303d005f9aac6e077259f41d94e2f5329d7f6` |
| Proposal length | 34 bytes, submitted inline in the referendum (no preimage deposit) |
| New runtime | Relaychain v1.8.0 `enjin_runtime.compact.compressed.wasm`, 2,122,534 bytes |
| Runtime code hash (blake2-256) | `0xb3c5e0dbd0e13b5936d8aead6cbcb505ddb71a671094190a7f08c7e4b2a5a0da` |
| Track | `root` (ID 0) |
| Dispatch origin | Root |
| Submission deposit | 0.025 ENJ |
| Decision deposit | 1,000,000 ENJ (refundable on decision) |
| Track periods | prepare 2 h · decide up to 14 days · confirm 24 h · minimum enactment 24 h |
| Enactment | `At(block 18,059,050)`, ≈ Monday 9 November 2026, 15:55 UTC → `system.UpgradeAuthorized {codeHash, checkVersion: true}` |
| Application | `system.applyAuthorizedUpgrade(code)` with the v1.8.0 runtime, feeless, submitted by Enjin for inclusion in block 18,059,100 (≈ 16:00 UTC); the code is written to `:code` in that block (`system.CodeUpdated`) and runs from block 18,059,101 |
| Effect | `spec_version` 1070 → 1080, `transaction_version` 18 → 19; `:code` blake2-256 = `0xb3c5e0dbd0e13b5936d8aead6cbcb505ddb71a671094190a7f08c7e4b2a5a0da` |
| Not used | `system.setCode(code)` directly, with the runtime as a 2,122,540-byte preimage. `authorizeUpgrade` keeps the proposal to 34 bytes that contain the code hash itself |

To verify: the referendum's proposal on [gov.enjin.cloud](https://gov.enjin.cloud) or Polkadot-JS Apps should be the encoded call data above, with proposal hash `0xaa04cc2c31d7cf2374399be8995303d005f9aac6e077259f41d94e2f5329d7f6`; its last 32 bytes are the runtime code hash. The runtime itself is published on-chain when it is applied, and after the upgrade the blake2-256 of the code stored at `:code` should equal `0xb3c5e0dbd0e13b5936d8aead6cbcb505ddb71a671094190a7f08c7e4b2a5a0da`.

## Voting and Enactment

This referendum is submitted on the root track on Monday 12 October 2026 with a decision period of up to 14 days and a 24-hour confirmation period. Enactment is fixed at block 18,059,050, which authorizes the code hash about five minutes ahead of the upgrade. The code is then applied in block 18,059,100, approximately Monday 9 November 2026 at 16:00 UTC, and v1.8.0 runs from the next block; the time is an estimate at the measured 6.00 seconds per block. The upgrade cannot happen before block 18,059,050. The Matrixchain upgrade follows one week later, on Monday 16 November 2026, under its own referendum.

## References

- Announcement: [Enjin Blockchain "Fort Canning" Upgrade](https://enjin.io/blog/enjin-blockchain-fort-canning-upgrade)
- Full changelog and technical notices: [Fort Canning changelog](https://enjin.io/changelog/fort-canning)
- Companion proposal: [GP-2026-11-10-01, Matrixchain Runtime Upgrade to v1.4.1](GP-2026-11-10-01-matrixchain-v1-4-1-upgrade.md)
