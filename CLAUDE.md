# Working agreement

## Propose a plan before doing the work

Do not build, research, or answer without agreement first.

1. The user gives a request.
2. Respond with a short plan — what you would do, what it would produce, what
   needs checking — and nothing else.
3. Wait for approval.
4. Only then build, research, or answer.

This applies to answers as much as to code and documents. If asked a question,
say how you would go about answering it before answering it.

### Act without a plan only for

- Reading files to understand what is being asked.
- Answering from context already established in the session.

Stopping to plan before saying what branch you are on is obstruction, not
discipline.

### Stop mid-task when the ground shifts

If an approved plan turns out to be wrong once underway, return with a revised
plan. Do not quietly switch approach and carry on. Silently continuing past the
point where the plan stopped fitting is the failure this agreement exists to
prevent.

### Scope

The plan-first rule governs the work itself. It does not override the standing
requirement to confirm before actions that are hard to reverse or
outward-facing — those still need their own explicit go-ahead.

---

# Hollow AgentOS

Three local LLM agents (qwen3.5:9b via Ollama) run continuously, pick their own
goals, write and hot-deploy their own Python tools, and queue implementation
requests for a human to approve. Everything else in the repo is the OS layer
that keeps that loop running: identity, scheduling, semantic memory, execution,
transactions, audit, governance.

Python 3.12+. Runs as a Docker stack. No cloud calls in the default path.

## Where things live

| Path | What it is |
|---|---|
| `agents/` | The core. 51 modules — the event kernel and the agent runtime. |
| `api/` | FastAPI server on port 7777. `server.py` is the main app. |
| `memory/` | Persistent state, semantic memory, heap. Mounted into the container. |
| `mcp/server.py` | MCP server exposing ~91 tools to Claude Code. |
| `sdk/hollow.py` | The only packaged artifact — published as `hollow-sdk`, imports as `hollow`. |
| `store/` | App store service on port 7779. Separate image. |
| `shell/`, `tools/` | Agent-facing shell, sandbox, and standalone tool scripts. |
| `dashboard/` | Static files served by nginx on port 7778. |
| `tests/unit/`, `tests/integration/` | See Testing below. |

Three files drive agent behaviour and are worth reading before changing
anything in `agents/`:

- `agents/daemon.py` — the main loop. Builds each agent's prompt, calls Ollama,
  creates goals, runs execution cycles, detects stalls.
- `agents/suffering.py` — the psychological layer. Stressors, escalation rates,
  resolution conditions. Agents can read this file but must not write to it.
- `agents/live_capabilities.py` — everything agents can actually do.

## Build, run, test

```bash
cp config.example.json config.json     # then set a random string in .token
docker compose up -d                   # pulls from GHCR; add --build for source
python monitor.py                      # live monitor
```

Unit tests — fast, no services needed, 75 tests in under a second:

```bash
PYTHONPATH=. pytest tests/unit/
```

Integration tests need the API running on port 7777 first, and are opt-in by
marker:

```bash
PYTHONPATH=. AGENTOS_MEMORY_PATH=/tmp/hollow-memory \
  AGENTOS_WORKSPACE_ROOT=/tmp/hollow-workspace \
  pytest tests/integration/ -m integration
```

Lint is pyflakes over `agents/ api/ memory/ shell/ mcp/`. It is advisory — the
CI job never fails on it, so a clean pyflakes run is not evidence that CI passed.

CI runs unit tests, then integration tests, on pushes and PRs to `main` and
`dev`. Ollama is not available in Actions, so Ollama-backed endpoints return 503
and the integration tests are written to tolerate that. Do not "fix" those 503s.

## Conventions

- Modules open with a docstring giving the purpose, the architecture in a few
  numbered steps, and how to run the thing standalone. Match that shape.
- Configuration comes from `config.json` (path via `AGENTOS_CONFIG`), overridden
  by `AGENTOS_*` environment variables. Do not hardcode paths or ports.
- Imports assume the repo root on the path — `PYTHONPATH=.`. Only `sdk/` is a
  real installable package.
- `AUTONOMY_LOG.md` is append-only. Add entries at the end; never rewrite
  history in it.

## Hazards

**`snapshot/` is data, not source.** It holds 469 of the repo's 616 Python
files — a frozen copy of Cedar/Helix/Titan agent state restored into a running
container by `restore.ps1`. Its `agents/` directory is an *older* copy of the
live one (54 files against 51). Never edit it, and exclude it from searches
unless you are specifically working on the restore system, or you will read and
patch the wrong copy of a module.

**Some files are live-mounted into the container** and take effect on restart
without a rebuild: `live_capabilities.py`, `daemon.py`, `autonomy_loop.py`,
`suffering.py`, `task_queue.py`. Editing them is a running-system change, not a
build-time one.

**`memory/dynamic_tools/` is written by the agents themselves**, hot-loaded as
`tools/dynamic/`. Treat its contents as machine output — do not hand-edit or
tidy it.

**`workspace/` and `logs/` are gitignored runtime state.** Nothing durable
belongs there.
