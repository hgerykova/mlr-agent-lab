# Plan

Progress and the next step. Per `AGENTS.md` "Keep the docs honest", move items to Done in the
same commit that completes them, and keep **Next step** to one concrete
action — if there are three candidates, one of them is the real next step
and the others are a backlog.

## Done

- 2026-10-14 — `hello.py` entry point, verified with `uv run hello.py` →
  `hello` (`6a74435`).
- 2026-10-14 — repo created public as `hgerykova/mlr-agent-lab`, `main`
  pushed. Commit author is the GitHub noreply alias, checked server-side via
  the API, so no institutional email is in public history.
- 2026-10-14 — working rules written: `AGENTS.md` (uv-only, read-first,
  verify-before-done, no secrets), `CLAUDE.md` as the loader.
- 2026-10-14 — docs scaffolded: `README.md`, `PLAN.md`, `RUNS.md`.
- 2026-10-14 — `AGENTS.md` expanded to 8 rules: added try-the-tools-first,
  don't-weaken-tests, keep-decisions-in-repo, and ask-before-publishing.
- 2026-10-14 — `.gitignore` added (secrets, caches, run artifacts, weights);
  verified it hides the `__pycache__/` that `import hello` had already
  created. Living docs now reference rules by name, not section number.

## Next step

**Start the uv project and write the first failing test**, so that "run
meaningful checks" points at a real check instead of an import smoke test:

```bash
uv init --no-readme
uv add --dev pytest
uv run pytest
```

Do this before any agent code, because a change made under "the check is a
print statement" is not verifiable in the sense the rules mean.

## Waiting on you

Not blockers I can clear myself:

1. **The actual goal.** `README.md` states an intent inferred from the repo
   name. A sentence on what kind of agent and what benchmark decides
   whether every following step is on target or just activity.
2. **Model id in `RUNS.md`.** I cannot read my own model name from inside
   the session — `/status` shows it to you. Entries carry an explicit
   `unknown` where I could not fill it.
3. **`pyproject.toml` metadata** — package name and Python floor, at
   `uv init` time. Default picks up the folder name and whatever Python is
   active; worth choosing deliberately once, cheaper than migrating.

## Backlog

Rough, unordered, and not commitments:

- First agent loop: one tool call, one model call, no framework.
- Deterministic seed + logging convention, so `RUNS.md` entries can point at
  an artifact rather than a screenshot.
- CI running the check on push — the rules are decoration until something
  enforces them.
