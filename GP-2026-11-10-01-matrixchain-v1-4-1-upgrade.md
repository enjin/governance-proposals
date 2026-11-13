# Governance Proposal: Enjin Matrixchain Runtime Upgrade to v1.4.3 ("Fort Canning")

Date: 2026-11-10

Proposal ID: GP-2026-11-10-01

Submitted by: Enjin

Published for advance notice: 6 October 2026

> **Revision, 13 November 2026.** This text first described v1.4.1. Referendum #22 authorized v1.4.3 (spec version 1043), the release that followed v1.4.1 and v1.4.2 with the fixes below, and this text now gives the v1.4.3 values and the referendum as it ran. The v1.4.1 text is in this file's history.

## Summary

This proposal authorizes the upgrade of the Enjin Matrixchain runtime from v1.3.1 (spec version 1031) to v1.4.3 (spec version 1043), codenamed Fort Canning. The referendum authorizes the new runtime's code hash, and the authorized code is then applied at about Matrixchain block 12,319,600, approximately Monday 16 November 2026 at 16:00 UTC. The upgrade brings OpenGov to the Matrixchain, streamlined marketplace listings and offers, token lending, ephemeral tokens, mint rate limits and attribute freezing. The Relaychain upgrade, GP-2026-10-12-01, enacted one week earlier, on 9 November.

> **Action required.** Every Matrixchain node must run client v1.4.1 or later before enactment, or it stops following the chain; v1.4.3 is recommended, as it carries upstream security fixes. Wallets, dependencies and exchanges that hard-code the transaction version must switch from 12 to 16 at enactment. Integrations that move multi-tokens over XCM (`xTokens`, `matrixXcm`, `ormlXcm`) must stop: those calls are removed. Fuel tank owners must reset permitted-call rules that named the retired custom utility pallet. Details are in the [changelog](https://enjin.io/changelog/fort-canning).

## Motion

Authorize runtime code hash `0xd54da55fa212e0de28f09476bff27724856d8bb488fcad61ae1a4a9bd550355b` (blake2-256 of `enjin-matrix_runtime.compact.compressed.wasm`, Matrixchain v1.4.3) by executing `system.authorizeUpgrade(0xd54da55fa212e0de28f09476bff27724856d8bb488fcad61ae1a4a9bd550355b)` with the Root origin, dispatched by a Democracy referendum. The referendum is proposed through `democracy.externalProposeMajority` and fast-tracked through `democracy.fastTrack`, the path used for the Bugis (v1.2.2, March 2025) and Sentosa (v1.3.1, December 2025) upgrades. `checkVersion` is true, so the code is verified to carry `spec_name` `matrix-enjin` and a higher `spec_version` before it can be applied.

## Changes

- **OpenGov.** v1.4.3 replaces the Democracy, Council and Technical Committee pallets with OpenGov (Referenda, Conviction Voting, Origins, Whitelist and the Enjin Fellowship) with nine tracks, so any account can submit a referendum and ENJ holders decide directly. Support is measured against the Relaychain's total issuance, floored at the Matrixchain's own, so a track needs the same ENJ support as on the Relaychain. Fellowship referenda take a 100 ENJ submission deposit. Pending Democracy proposals are cancelled, their deposits refunded and vote locks released by migration. From v1.4.3, Matrixchain upgrades are proposed as OpenGov referenda on the root track, or whitelisted by the Enjin Fellowship and decided on the whitelisted-caller track (15-minute preparation, up to 3 days of voting, 6-hour confirmation, support falling from 1% to 0.15%), with their texts published in this repository before submission. This completes the 2026 upgrade cycle: Kallang (Relaychain runtime 1.7.0, referendum #4, enacted 18 May 2026) made the Relaychain community-governed and removed its sudo pallet, and its announcement stated that the Matrixchain would follow.
- **Treasury.** Spending from the Matrixchain community pool is suspended in favour of the Relaychain treasury: there are no treasury, tipper or spender tracks, and the bounties pallet is removed. The upgrade's migrations pay 6.35 ENJ (the liquidity-pool token collection) and 0.1 ENJ (the infusion pot) from the community pool.
- **Multi-token XCM removed.** The `xTokens`, `matrixXcm`, `ormlXcm` and `unknownTokens` pallets are removed, so XCM no longer carries multi-tokens and pays fees only in the Relaychain's ENJ.
- **Marketplace listings and offers.** Offers can be partly accepted, listings that can no longer be completed can be cancelled by anyone, and expired listings settle on-chain instead of through an offchain worker, so settlement no longer depends on a collator running it. A listing is cancelled and refunded if one of its tokens is created again under the same id. Open counter-offers are refunded at the upgrade, and the marketplace pauses for up to 30 minutes while listings migrate.
- **Token lending and ephemeral tokens.** Owners can lend non-fungible tokens for a fixed term, and tokens can be minted with an expiry up to one year ahead.
- **Mint rate limits.** Collection owners can cap minting per period, and a mint into a token with no supply counts against the cap. Loosening a limit takes 72 hours, giving time to react if a minting key is compromised.
- **Attribute freezing.** Collection, token and token group attributes can be frozen permanently.
- **Proof-size reclaim.** Unused proof budget is returned to the block after each transaction. This is the change that requires the client upgrade.
- **Fees.** Weights are regenerated on reference hardware. Most fees move by less than 10%, and most fuel tank calls cost 2 to 10 times more.
- **Polkadot SDK stable2606**.
- Bug fixes and security improvements, including the findings of the October 2026 code audit.

## On-Chain Execution

| Field | Value |
|---|---|
| Network | Enjin Matrixchain (parachain 1000 on the Enjin Relaychain) |
| Runtime at drafting | `matrix-enjin/1031`, transaction version 12 |
| Call | `system.authorizeUpgrade(0xd54da55fa212e0de28f09476bff27724856d8bb488fcad61ae1a4a9bd550355b)` (pallet 0, call 9) |
| Encoded call data | `0x0009d54da55fa212e0de28f09476bff27724856d8bb488fcad61ae1a4a9bd550355b` |
| Preimage hash (blake2-256) | `0x9f3b9d8968184f7ea5ad28b6386f59a2b0162a2970677c7852b0e12ad71fc1f1` |
| Preimage length | 34 bytes (deposit 85.25 ENJ, refundable) |
| New runtime | Matrixchain v1.4.3 (`enjin-matrix_runtime.compact.compressed.wasm`, 1,873,115 bytes; relay `max_code_size` 3,145,728) |
| Code hash (blake2-256 of the runtime) | `0xd54da55fa212e0de28f09476bff27724856d8bb488fcad61ae1a4a9bd550355b` |
| Proposal origin | `democracy.externalProposeMajority`; threshold SimpleMajority |
| Fast-track origin | `democracy.fastTrack(0x9f3b9d8968184f7ea5ad28b6386f59a2b0162a2970677c7852b0e12ad71fc1f1, 21000, 7000)`, Technical Committee motion #39, 3 of 3 ayes |
| Referendum | #22, block 12,260,236 to 12,281,236 (21,000 blocks, 12 November 00:19 UTC to 13 November 15:52 UTC), approved |
| Dispatch | Scheduler, at block 12,288,236 (voting end + 7,000 blocks, ≈ Saturday 14 November 05:00 UTC), origin Root → `system.UpgradeAuthorized {codeHash, checkVersion: true}` |
| Application | `system.applyAuthorizedUpgrade(code)`, permissionless, submitted targeting block 12,319,600 (≈ Monday 16 November 2026, 16:00 UTC, at 6.77 s per block); effective about 20 Relaychain blocks (≈2 minutes) after inclusion |
| Effect | `spec_version` 1031 → 1043, `transaction_version` 12 → 16; Relaychain `paras.currentCodeHash(1000)` → `0xd54da55fa212e0de28f09476bff27724856d8bb488fcad61ae1a4a9bd550355b` |
| Not used | `system.setCode` directly: its preimage would require a deposit of about 4.8 million ENJ under current parameters |

To verify: compare the preimage hash above with the scheduled call's hash (`scheduler.agenda(12288236)`, a `Lookup` of length 34), and after enactment compare the code hash with the blake2-256 of the runtime stored at `:code` and with `paras.currentCodeHash(1000)` on the Relaychain. The "authorizeUpgrade hash" on the GitHub release page is computed for a different call layout and does not match the preimage hash; use the "Blake2-256 hash" line, which is the code hash above.

## Voting and Enactment

This proposal was published on 6 October 2026, giving at least four weeks of notice. Referendum #22 opened on Thursday 12 November 2026, two days later than first planned, after the v1.4.3 release, and was fast-tracked to a 21,000-block voting period, about 39.5 hours. It closed on Friday 13 November at block 12,281,236 and was approved. The upgrade is authorized at block 12,288,236 and the code applied on Monday 16 November as first planned, so that the new runtime takes effect at about Matrixchain block 12,319,600, approximately 16:00 UTC. The block is a target, re-confirmed closer to the date.

## References

- Announcement: [Enjin Blockchain "Fort Canning" Upgrade](https://enjin.io/blog/enjin-blockchain-fort-canning-upgrade)
- Full changelog and technical notices: [Fort Canning changelog](https://enjin.io/changelog/fort-canning)
- Release: [Matrixchain v1.4.3](https://github.com/enjin/matrixchain/releases/tag/v1.4.3)
- Companion proposal: [GP-2026-10-12-01, Relaychain Runtime Upgrade to v1.8.0](GP-2026-10-12-01-relaychain-v1-8-0-upgrade.md)
