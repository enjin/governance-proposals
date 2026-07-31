# Enjin Governance Proposals

Canonical texts of governance proposals for the Enjin Relaychain. Each proposal is one markdown file, named by its proposal ID. Once a referendum is submitted on-chain, the file for its proposal is considered final; subsequent edits are limited to status updates.

Vote and browse live referenda at [gov.enjin.cloud](https://gov.enjin.cloud).

## Proposals

| ID | Title | Track | Status |
|---|---|---|---|
| [GP-2026-08-03-01](GP-2026-08-03-01-validator-expansion.md) | Expansion of the Enjin Relaychain Active Validator Set from 25 to 35 | Staking Admin | Pending submission |

## Writing a proposal

Start from [TEMPLATE.md](TEMPLATE.md). Conventions:

- **Proposal ID:** `GP-YYYY-MM-DD-NN`, dated to the planned submission date, `NN` incrementing within a day.
- **File name:** `GP-YYYY-MM-DD-NN-short-slug.md`.
- Keep motions precise: name the exact call, origin, and amounts. Claims about live network state should cite their source and date.
- On-chain referenda reference their text by URL. Use this file's raw URL (`https://raw.githubusercontent.com/enjin-misc/governance-proposals/main/<file>.md`) so third parties can verify the text against the on-chain reference.
