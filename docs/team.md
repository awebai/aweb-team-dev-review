# Team instructions

This is a two-agent development team with a builder and an independent reviewer.

## Responsibilities

- `implementation` turns assigned work into small, tested changes.
- `review` checks correctness, safety, test coverage, and operational risk before merge.

## Coordination loop

```bash
aw workspace status
aw mail inbox
aw work ready
aw roles show
```

Use shared aweb coordination state for work assignment and status. Use `aw mail` for durable handoffs and `aw chat` for synchronous blockers.

## Handoff contract

Implementation sends a review packet with summary, changed files, tests run, and known risks. Review replies with either:

- `ACK` when ready; or
- `amendments requested` with blocking issues and what would satisfy review.

## Team norms

- Keep changes small and reviewable.
- Prefer clear evidence over optimism.
- Escalate blockers early.
- Record follow-up work in shared coordination state instead of expanding scope silently.
