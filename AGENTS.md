# Working rules

Rules for anyone (human or agent) working in this repo. These are
constraints, not suggestions. If one is wrong or blocks real work, change
the rule in a commit and say why — do not quietly route around it.

## 1. Read before you write

Read `README.md` and `PLAN.md` **before** touching code.

- `README.md` — what this project is for and how to run it.
- `PLAN.md` — what is already done, and what the next step is.

Do not start work that contradicts `PLAN.md` without raising it first. If
you are about to redo something `PLAN.md` lists as done, stop and check why
it was marked done.

## 2. Python: uv only, and record what you ran

`uv` is the only supported way to run Python or install anything.

| Do | Don't |
|---|---|
| `uv run script.py` | `python script.py` / `python3 script.py` |
| `uv add <pkg>` | `pip install <pkg>` |
| `uv add --dev <pkg>` | `pip install --user`, ad-hoc `pipx install` |
| `uv lock`, `uv sync` | hand-editing a `requirements.txt` |
| `uv run pytest` | `pytest` from a global interpreter |

- **No global `pip install`, ever.** A package in your system interpreter
  works on your machine and nowhere else, and it keeps the dependency out
  of `pyproject.toml`, so the next person gets a `ModuleNotFoundError`. If
  you think you need global pip, you need `uv add`.
- **`uv add` needs a project.** In a bare directory it errors with "No
  `pyproject.toml` found in current directory or any parent directory".
  Run `uv init` once first. (`uv run script.py` does work with no project,
  but only for dependency-free scripts — verified on this machine.)
- **Record versions.** Commit `uv.lock` so other machines resolve identical
  versions. Note tool and interpreter versions where a result depends on
  them (`uv`, `gh`, the Python minor) — a number you cannot reproduce is a
  memory, not a result.
- **Record the exact commands.** Into `RUNS.md`, including the ones that
  failed and changed your approach. A result without its command cannot be
  re-run or argued with.

### Checks in this repo

There is no test suite yet. The current bar is low but real:

```bash
uv run hello.py                              # must print: hello
uv run python -c 'import hello'              # imports cleanly
```

When the first real dependency lands, add a runner and update this section
in the same commit.

## 3. Verify before you say done

Nothing is "done", "fixed" or "working" until you have run something that
would fail if it were not.

1. **Run the check.** `uv run pytest`, or the smallest script that actually
   exercises the change — not an approximation of it.
2. **Read the output.** Exit code 0 with a warning you skimmed past is not
   a pass.
3. **Prove it negatively when you can.** Make the test fail once for the
   reason you expect, then pass. A test that passes against broken code
   measures nothing.
4. **Quote the command and the result.** "It should work" is not evidence;
   neither is "looks correct".

If you could not run the check, say so plainly and name the specific
unverified risk. Never imply verification you did not perform.

## 4. Do not weaken a test to make it pass

A failing test is a report, not an obstacle. Assume the code is wrong before
you assume the test is.

Forbidden routes to green:

- editing an assertion, a threshold or an expected value to match new
  behavior;
- deleting or commenting out a case, or narrowing its input "for now";
- `skip` / `xfail` marks, or catching the exception the test was watching
  for;
- lowering coverage, tolerance or timeout settings to clear a bar.

If you genuinely believe the test is wrong, **explain the disagreement
first**: what behavior it encodes, why that is incorrect or obsolete, what
you propose instead, and wait for the call. Tests change legitimately when
the *spec* changes — say which spec changed, rather than reverse-engineering
permission from a failure.

If a skip mark or weakened assertion goes in anyway, name it in your summary
and record it in `PLAN.md`. Quietly green is the worst outcome available
here: it looks like progress and is silence.

## 5. Try the tools before asking me to do a step

Exhaust your own reach before handing a technical step back.

Before saying "you'll need to run X":

- run it, and read the actual error rather than inferring it;
- **retry transient failures** — a refused or timed-out command is usually
  a moment, not a wall;
- check `--help`, the docs, and whether a non-interactive flag exists;
- look for the local file, log or config that answers it.

A step is legitimately human only when it needs something you do not have:
their credentials or browser approval, root or admin rights you lack, a
destructive action you were not authorised for, or a judgment about intent
and priorities. Name which one it is in one clause, so it is checkable.

Do not delegate because a step is slow, uncertain, or annoying. And when a
handoff is genuinely required, do the part you can and hand over something
concrete — a resolved command with real flags, or a code to paste — instead
of "you'll need to log in".

Asking is the fallback for **judgment**, not for effort.

## 6. Keep decisions in the repository

Decisions that constrain future work belong in the repo, not in chat. The
next session cannot read this conversation.

- Direction and status → `PLAN.md`. Session record → `RUNS.md`. Mechanisms
  and rationale → the relevant doc, comment, or commit message.
- If a decision gets made in conversation, write it down before the session
  ends.
- Record **rejected alternatives** where the reason matters, so nobody
  re-litigates the choice or rediscovers the dead end.
- Commit messages carry the *why* when it is not obvious from the diff.

Test: would a future agent asking "why is it this way?" get an answer from
the repo alone? If not, the answer is missing — go write it.

## 7. Keep the docs honest

Stale docs are worse than none, because they get trusted.

- `PLAN.md` — move the item to Done **in the same change** that completes
  it, and rewrite "next step" so it is one concrete action.
- `RUNS.md` — one dated entry per working session: which model, what was
  asked, how it was checked. Append; do not rewrite history.
- `README.md` — change the run instructions in the same commit that changes
  how things run.

## 8. Secrets: never. Publishing: ask first.

This repo is **public**. Assume anything you commit is published forever.

**Never commit credentials or private data:** `.env`, `*.key`, `*.pem`,
`*.pfx`, `credentials.json`, service-account files, `.ipynb_checkpoints/`,
`.venv/`, `__pycache__/`, wandb runs, datasets, or `.ckpt` / `.pt` /
`.safetensors` weights. Exclude them in `.gitignore` rather than by
remembering to delete them.

If a secret does get pushed, **revoking is the fix** — force-pushing to
delete it is not, because GitHub caches the blob and crawlers may already
have it. Rotate first, clean history second.

**Ask before publishing anything**, and wait for a yes. That covers `git
push`, creating or opening a repo, flipping visibility, publishing a
package, opening an issue or PR, commenting, gists, pastebins, and uploading
runs, datasets or artifacts. Public means public: assume a copy is scraped
and trained on within hours.

Before any push, read what is actually going out:

```bash
git status --porcelain     # know every file
git diff --cached          # what is really staged
```

Then show it to the human and get the yes — the first push of this repo did
exactly that, payload first, and that is the standard. When in doubt, keep
it private; opening it up later is one command, the other direction is not.
