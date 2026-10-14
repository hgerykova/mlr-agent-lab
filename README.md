# mlr-agent-lab

Experiments with machine-learning agents. Early stage — the working rules in
[`AGENTS.md`](AGENTS.md) are currently the most substantial thing here.

## Goal

Experiment with LLM/RL agents and keep each run legible after the fact.

**This sentence is inferred from the repo name, not written down by you.**
Replace it with the real objective — what kind of agent, against what
benchmark — as soon as you can. A README can be vague about many things; the
goal should not be one of them, because it is what tells you whether a step
was progress or just activity.

## Status

One script and a set of conventions. No model, training loop, or evaluation
harness yet, and no `pyproject.toml`. See [`PLAN.md`](PLAN.md) for what
exists and what comes next.

## Requirements

- [`uv`](https://docs.astral.sh/uv/) on your PATH — checked here with uv
  0.13.1 (13 Oct 2026).

No other Python setup is supported. Read the "Python: uv only" rule in
`AGENTS.md` before reaching for `pip`.

## Run it

```bash
uv run hello.py
```

Expected output, exactly:

```
hello
```

This works without a `pyproject.toml` because `hello.py` imports nothing.
The moment something needs a package, start a project:

```bash
uv init --no-readme     # creates pyproject.toml; README.md already exists
uv add <package>
uv run <script>
```

## Check it

```bash
uv run hello.py                       # must print: hello
uv run python -c 'import hello'       # module imports cleanly
```

These are the checks referenced by the *verify before you say done* rule in
`AGENTS.md`. There is no test runner yet; when one is added, wire it up and
update this section in the same commit.

## Layout

| File | Purpose |
|---|---|
| `AGENTS.md` | Working rules — read-first, uv-only, verify-before-done |
| `CLAUDE.md` | Claude Code entrypoint; loads `AGENTS.md` |
| `README.md` | This file — goal and how to run things |
| `PLAN.md` | What is done, and the single next step |
| `RUNS.md` | Dated log: model, request, how it was checked |
| `hello.py` | Placeholder entry point |
