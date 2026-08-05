# Research run state

Resume point for this research thread. Update after every phase.

## Current status: scope changed — playbook delivered

The planned clean re-run of the STR research was **superseded**. The user
reframed the deliverable: the short-term rental is one use case, and what they
want is a general playbook for choosing tools when building custom software for
small businesses.

That reframe largely dissolved the Phase 0 blocker. Precise vendor pricing was
load-bearing for an STR *buying* decision; in a playbook it is a footnote, and
the findings that generalise best (Apple Guideline 4.2, CVE-2025-48757, Bubble's
metering, the Lovable rendering change) were already verified against primary
sources.

## Deliverables

| Document | Path | Status |
|---|---|---|
| Small business build playbook | `docs/research/small-business-build-playbook.html` | delivered, commit 94564a5 |
| STR direct-booking report | `docs/research/str-direct-booking-stack-report.html` | delivered, commit 73ffedf |
| Source reachability pre-test | `docs/research/str-stack/00-scope.md` | complete, commit 812ad77 |

## Phases of the abandoned clean re-run

| Phase | Status |
|---|---|
| 0 — Scope + reachability pre-test | complete — see `00-scope.md` |
| 1 — Search sweep | not started, superseded |
| 2 — Fetch + extract | not started, superseded |
| 3 — Verify | not started, superseded |
| 4 — Synthesis | delivered instead as the playbook |

## Still open, if the thread is picked up again

Constraints recorded in `00-scope.md` remain in force unless the environment
changes:

- 15 of 16 priority vendor domains blocked by egress policy
- Ultimate Web Scraper is read-only in this session (`create_extractions: false`)
  despite 9,686 credits being available
- Consequently all pricing in both documents is banded, not quoted

Unresolved factual gaps, all requiring vendor-page access:

- Lodgify tier boundary — does the booking fee drop at the ~$40 tier or ~$103?
- Guesty Lite — free for up to 3 listings, or ~$34 per listing?
- NSW STRA registration fees and the 180-day cap detail (needs NSW Planning Portal)
- Median / Natively pricing (current source is a competitor's page)
- Retool, Softr, Glide, Noloco, FlutterFlow pricing never independently researched
