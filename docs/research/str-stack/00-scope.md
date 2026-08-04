# Phase 0 — Scope and source-reachability pre-test

**Run:** clean re-run of the STR direct-booking stack research
**Date:** 2026-08-04
**Status:** BLOCKED — see "Blocker" below

## Purpose of this phase

Before spending any WebSearch budget, establish which sources are actually
reachable from this environment. The first run discovered the egress wall one
page at a time, *after* paying for the searches, which is why ~30 of 48 sources
returned HTTP 403 and most pricing in the resulting report is tagged
"Indicative" rather than verified.

## Reachability test results

Direct `curl` through the agent proxy, 12s timeout, 2026-08-04.

| Host | Status |
|---|---|
| developer.apple.com | **200 — reachable** |
| lodgify.com | blocked |
| ownerrez.com | blocked |
| beds24.com | blocked |
| smoobu.com | blocked |
| hostaway.com | blocked |
| guesty.com | blocked |
| bubble.io | blocked |
| lovable.dev | blocked |
| flutterflow.io | blocked |
| median.co | blocked |
| buildnatively.com | blocked |
| webflow.com | blocked |
| framer.com | blocked |
| planning.nsw.gov.au | blocked |
| stripe.com | blocked |

1 of 16 priority sources reachable. `WebFetch` follows the same policy
(confirmed separately against docs.lovable.dev → 403).

Per `/root/.ccr/README.md`, a 403 from the proxy means "the destination host is
not allowed by your organization's egress policy for this session. Do not retry
or route around it — report the blocked host." These are policy denials, not
transient failures.

## Fallback route also unavailable

The Ultimate Web Scraper MCP runs cloud browsers outside this container and
would have been the correct workaround for vendor pricing pages. It is not
usable here:

```
workspace: CrackersSumo's Workspace   plan: Pro
credits:   9,686 of 10,000 available
access:    run_automations: false     create_extractions: false
```

Read-only access. Not a credits limitation — a permissions one. Only
`list_tables`, `get_table`, `query_table`, `get_run`, `list_runs`,
`export_table` and the docs tools are exposed to this session.

## Blocker

A "full clean re-run" cannot improve on the first run's **evidence quality**,
because the limiting factor is egress policy, not research method. Any re-run
would again be built almost entirely on WebSearch result snippets plus the one
allowlisted domain.

What a re-run *can* still improve:

- **Durability** — phased, committed after each step; immune to VM reclamation
- **Search efficiency** — capped per angle instead of burning 200/200
- **Honest verification** — separating REFUTED from UNVERIFIABLE, which the
  first run conflated and which made its statistics misleading
- **Coverage gaps** — FlutterFlow was never researched at all

What it cannot improve without an egress-policy change:

- Verifying vendor pricing against vendors' own pages
- Resolving the Lodgify tier / booking-fee conflict
- Resolving the Guesty Lite pricing contradiction
- Confirming NSW STRA figures against the NSW Planning Portal
- Replacing the motivated competitor source for Median/Natively pricing

## Options put to the user

1. Proceed with a methodology-only re-run, accepting snippet-grade sourcing
2. Widen the environment's network policy, then re-run with real source access
3. Enable `create_extractions` on the scraper workspace and route fetching there
4. Keep the existing report and verify the handful of disputed prices manually

Awaiting decision.
