# Governance Proposal: Enjin Matrixchain Runtime Upgrade to v1.4.1 ("Fort Canning")

Date: 2026-11-10

Proposal ID: GP-2026-11-10-01

Submitted by: Enjin

Published for advance notice: 6 October 2026

## Summary

This proposal authorizes the upgrade of the Enjin Matrixchain runtime from v1.3.1 (spec version 1031) to v1.4.1 (spec version 1041), codenamed Fort Canning. The referendum authorizes the new runtime's code hash, and the authorized code is then applied at about Matrixchain block 12,331,000, approximately Monday 16 November 2026 at 16:00 UTC. The upgrade brings OpenGov to the Matrixchain, streamlined marketplace listings and offers, token lending, ephemeral tokens, mint rate limits and attribute freezing. The Relaychain upgrade, GP-2026-10-12-01, enacts one week earlier, on 9 November.

> **Action required.** Every Matrixchain node must run client v1.4.1 or later before enactment, or it stops following the chain. Wallets, dependencies and exchanges that hard-code the transaction version must switch from 12 to 14 at enactment. Fuel tank owners must reset permitted-call rules that named the retired custom utility pallet. Details are in the [changelog](https://enjin.io/changelog/fort-canning).

## Motion

Authorize runtime code hash `0x6a9d01c6fb9bfcc8f86a1d78e3096e562755ab1ca8761eebd1b6efb64c79c2bb` (blake2-256 of `enjin-matrix_runtime.compact.compressed.wasm`, Matrixchain v1.4.1) by executing `system.authorizeUpgrade(0x6a9d01c6fb9bfcc8f86a1d78e3096e562755ab1ca8761eebd1b6efb64c79c2bb)` with the Root origin, dispatched by a Democracy referendum. The referendum is proposed through `democracy.externalProposeMajority` and fast-tracked through `democracy.fastTrack`, the path used for the Bugis (v1.2.2, March 2025) and Sentosa (v1.3.1, December 2025) upgrades. `checkVersion` is true, so the code is verified to carry `spec_name` `matrix-enjin` and a higher `spec_version` before it can be applied.

## Changes

- **OpenGov.** v1.4.1 replaces the Democracy, Council and Technical Committee pallets with OpenGov (Referenda, Conviction Voting, Origins, Whitelist and the Enjin Fellowship) with fifteen tracks, so any account can submit a referendum and ENJ holders decide directly. Deposits and support floors match the Relaychain values in GP-2026-10-12-01. Pending Democracy proposals are cancelled, their deposits refunded and vote locks released by migration. From v1.4.1, Matrixchain upgrades are proposed as OpenGov referenda on the root track, or whitelisted by the Enjin Fellowship and decided on the whitelisted-caller track (15-minute preparation, up to 3 days of voting, 10-minute confirmation, 0.25% support floor), with their texts published in this repository before submission. This completes the 2026 upgrade cycle: Kallang (Relaychain runtime 1.7.0, referendum #4, enacted 18 May 2026) made the Relaychain community-governed and removed its sudo pallet, and its announcement stated that the Matrixchain would follow.
- **Marketplace listings and offers.** Offers can be partly accepted, listings that can no longer be completed can be cancelled by anyone, and expired listings settle on-chain instead of through an offchain worker, so settlement no longer depends on a collator running it. Open counter-offers are refunded at the upgrade, and the marketplace pauses for up to 30 minutes while listings migrate.
- **Token lending and ephemeral tokens.** Owners can lend non-fungible tokens for a fixed term, and tokens can be minted with an expiry up to one year ahead.
- **Mint rate limits.** Collection owners can cap minting per period. Loosening a limit takes 72 hours, giving time to react if a minting key is compromised.
- **Attribute freezing.** Collection, token and token group attributes can be frozen permanently.
- **Proof-size reclaim.** Unused proof budget is returned to the block after each transaction. This is the change that requires the client upgrade.
- **Polkadot SDK stable2606**.
- Bug fixes and security improvements.

## On-Chain Execution

| Field | Value |
|---|---|
| Network | Enjin Matrixchain (parachain 1000 on the Enjin Relaychain) |
| Runtime at drafting | `matrix-enjin/1031`, transaction version 12 |
| Call | `system.authorizeUpgrade(0x6a9d01c6fb9bfcc8f86a1d78e3096e562755ab1ca8761eebd1b6efb64c79c2bb)` (pallet 0, call 9) |
| Encoded call data | `0x00096a9d01c6fb9bfcc8f86a1d78e3096e562755ab1ca8761eebd1b6efb64c79c2bb` |
| Preimage hash (blake2-256) | `0x9304ebee2b729f9d6f5ceaf20fdff0d429e3498a362dbf95e914a98af9f58196` |
| Preimage length | 34 bytes (deposit 85.25 ENJ, refundable) |
| New runtime | Matrixchain v1.4.1 (`enjin-matrix_runtime.compact.compressed.wasm`, 1,912,444 bytes; relay `max_code_size` 3,145,728) |
| Code hash (blake2-256 of the runtime) | `0x6a9d01c6fb9bfcc8f86a1d78e3096e562755ab1ca8761eebd1b6efb64c79c2bb` |
| Proposal origin | `democracy.externalProposeMajority`; threshold SimpleMajority |
| Fast-track origin | `democracy.fastTrack(0x9304ebee2b729f9d6f5ceaf20fdff0d429e3498a362dbf95e914a98af9f58196, votingPeriod, 15)` |
| Voting period | about 38,300 blocks ≈ 72 hours at 6.76 s per block (block count set at submission from the live block rate) |
| Dispatch | Scheduler, at voting end + 15 blocks, origin Root → `system.UpgradeAuthorized {codeHash, checkVersion: true}` |
| Application | `system.applyAuthorizedUpgrade(code)`, permissionless, submitted targeting block 12,331,000 (≈ Monday 16 November 2026, 16:00 UTC); effective about 20 Relaychain blocks (≈2 minutes) after inclusion |
| Effect | `spec_version` 1031 → 1041, `transaction_version` 12 → 14; Relaychain `paras.currentCodeHash(1000)` → `0x6a9d01c6fb9bfcc8f86a1d78e3096e562755ab1ca8761eebd1b6efb64c79c2bb` |
| Not used | `system.setCode` directly: its preimage would require a deposit of about 4.8 million ENJ under current parameters |

To verify: compare the preimage hash above with the referendum's proposal hash on-chain (`democracy.referendumInfoOf`), and after enactment compare the code hash with the blake2-256 of the runtime stored at `:code` and with `paras.currentCodeHash(1000)` on the Relaychain.

## Voting and Enactment

This proposal is published on 6 October 2026, giving at least four weeks of notice. The referendum opens on Tuesday 10 November 2026, the day after the Relaychain upgrade, and runs for about 72 hours, to Friday 13 November. If it passes, the authorized code is applied on Monday 16 November, so that the new runtime takes effect at about Matrixchain block 12,331,000, approximately 16:00 UTC. The block is a target, re-confirmed closer to the date.

## References

- Announcement: [Enjin Blockchain "Fort Canning" Upgrade](https://enjin.io/blog/enjin-blockchain-fort-canning-upgrade)
- Full changelog and technical notices: [Fort Canning changelog](https://enjin.io/changelog/fort-canning)
- Companion proposal: [GP-2026-10-12-01, Relaychain Runtime Upgrade to v1.8.0](GP-2026-10-12-01-relaychain-v1-8-0-upgrade.md)
