# Enjin Governance Proposals

Canonical texts of governance proposals for the Enjin Blockchain (Relaychain and Matrixchain). Each proposal is one markdown file, named by its proposal ID. Once a referendum is submitted on-chain, the file for its proposal is considered final; subsequent edits are limited to status updates.

Vote and browse live referenda at [gov.enjin.cloud](https://gov.enjin.cloud).

## Proposals

| ID | Title | Track | Status |
|---|---|---|---|
| [GP-2026-08-03-01](GP-2026-08-03-01-validator-expansion.md) | Expansion of the Enjin Relaychain Active Validator Set from 25 to 35 | Root | Approved (referendum #10); stage 1 enacted (27 validators), stages 2-4 scheduled |
| [GP-2026-10-12-01](GP-2026-10-12-01-relaychain-v1-8-0-upgrade.md) | Enjin Relaychain Runtime Upgrade to v1.8.0 ("Fort Canning") | Root | Approved (referendum #16), enacted 9 Nov |
| [GP-2026-11-10-01](GP-2026-11-10-01-matrixchain-v1-4-1-upgrade.md) | Enjin Matrixchain Runtime Upgrade to v1.4.3 ("Fort Canning") | Matrixchain Democracy | Approved, application 16 Nov |

## Writing a proposal

Start from [TEMPLATE.md](TEMPLATE.md). Conventions:

- **Proposal ID:** `GP-YYYY-MM-DD-NN`, dated to the planned submission date, `NN` incrementing within a day.
- **File name:** `GP-YYYY-MM-DD-NN-short-slug.md`.
- Keep motions precise: name the exact call, origin, and amounts. Claims about live network state should cite their source and date.
- On-chain referenda reference their text by URL. Use this file's raw URL (`https://raw.githubusercontent.com/enjin/governance-proposals/master/<file>.md`) so third parties can verify the text against the on-chain reference.
