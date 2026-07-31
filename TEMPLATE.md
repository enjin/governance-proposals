# Governance Proposal: <Title — what the proposal does, in one line>

Date: YYYY-MM-DD

Proposal ID: GP-YYYY-MM-DD-NN

Submitted by: <Entity or person accountable for the proposal>

## Summary

Three to six sentences. What is proposed, what this specific referendum enacts, and how it relates to any larger plan. A reader should understand the decision from this paragraph alone.

## Motions

Motion 1

The precise on-chain action: the exact call (e.g. `pallet.call(args)`), the origin/track it executes under, and when it takes effect. One motion per distinct action.

Motion 2 (optional)

Additional actions, or a non-binding endorsement of a broader plan or schedule. Mark clearly which motions execute on-chain and which are signals.

## Rationale

Numbered points. Each states one reason and backs it with verifiable facts (on-chain data with date/era, links). Avoid claims a voter cannot check.

## Impact / Economics

What changes for stakeholders (holders, stakers, validators, builders), with numbers where possible. State data sources and the date they were read. If funds are involved: amounts, source, recipient, schedule, and what happens to unspent funds.

## Risks and Considerations

Bullet the material risks, what mitigates each, and what is explicitly out of scope. Include reversibility: how this decision can be adjusted or undone if needed.

## Voting and Enactment

Keep this brief: the track, the planned submission date, and when the change takes force. If a deterministic in-force date matters, fix enactment with `At(<block>)` rather than `After(...)` and state the target date. Operational detail (submission extrinsics, deposits, block computation) belongs in your own runbook, not the proposal.

## Source and Recipient

Only for proposals that move funds: source of funds and recipient account. Omit this section entirely otherwise.
