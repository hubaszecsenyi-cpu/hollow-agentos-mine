# Research run state

Resume point for the STR direct-booking stack research. Update after every phase.

| Phase | Status | Output |
|---|---|---|
| 0 — Scope + reachability pre-test | **complete** | `00-scope.md` |
| 1 — Search sweep (capped per angle) | not started | `01-search-*.md` |
| 2 — Fetch + extract, batches of ~8 | not started | `02-claims-NN.md` |
| 3 — Verify load-bearing claims | not started | `03-verdicts.md` |
| 4 — Synthesis | not started | updated report |

## Current status: BLOCKED at Phase 0

Egress policy blocks 15 of 16 priority sources; the scraper fallback is
read-only in this session. A re-run can improve method and durability but not
evidence quality. See "Blocker" in `00-scope.md`. Awaiting a decision on the
four options listed there.

## Prior run, for reference

The completed first-pass report lives at
`docs/research/str-direct-booking-stack-report.html` (commit 73ffedf).
Its raw agent results are in the session's workflow journal at
`subagents/workflows/wf_00bd41a4-e4b/journal.jsonl` — **ephemeral**, lost when
this container is reclaimed. 127 agent results, 48 sources, 73 verified claims.
Nothing in this directory depends on that journal surviving.
