# Runs

One dated entry per working session. Three fields, and the third is the
point: *how it was checked*. An entry that records a request and a result
but no check is a diary entry, not a lab note — it cannot be re-examined or
falsified later.

Append. Do not rewrite past entries, even wrong ones; correct them by
appending a note saying what was wrong.

Template:

```markdown
## YYYY-MM-DD — <short title>

**Model:** <id from /status>
**Requested:** <what was asked for, in the asker's terms>
**Changed:** <files, commit sha>
**Checked:** <exact command run, and the output it produced>
**Unverified:** <what was claimed but not proved>
```

---

## 2026-10-14 — install tooling, create repo, add working rules

**Model:** unknown — not readable from inside the session. `/status` shows
it to you; fill it in if you want the entry complete. Two sessions today
(gh/uv install, then this repo's setup) may have used different models.

**Requested:** install `gh` and `uv` without admin rights; create a public
repo from this folder showing the payload before push; write `AGENTS.md`
working rules with `CLAUDE.md` loading it, plus `README.md`, `PLAN.md`,
`RUNS.md`.

**Changed:** `hello.py` (`6a74435`); `AGENTS.md`, `CLAUDE.md`, `README.md`,
`PLAN.md`, `RUNS.md` (uncommitted as of this entry). Binaries in
`~/.local/bin/gh.exe`, `uv.exe`, `uvx.exe`, `uvw.exe`.

**Checked:**

- `gh --version` → `gh version 2.102.0`; `uv --version` → `uv 0.13.1`. Both
  SHA256-verified against published digests before use.
- Public visibility, anonymously: `curl https://api.github.com/repos/
  hgerykova/mlr-agent-lab` → `"private": false`; `raw.githubusercontent.com`
  with no token returned `hello.py` verbatim.
- Commit author, server-side: `gh api repos/hgerykova/mlr-agent-lab/commits`
  → `hgerykova <159055653+hgerykova@users.noreply.github.com>`. Confirms the
  institutional address is absent from public history.
- `uv run hello.py` → `hello`, exit 0.
- `uv add iniconfig` in a bare temp dir → `error: No pyproject.toml found`.
  This is why `AGENTS.md` §2 documents `uv init` as a prerequisite rather
  than implying `uv add` works anywhere.
- `uv run python -c 'import hello'` → exit 0.
- `uv run hello.py` with no `pyproject.toml` present → works, and created no
  files. Documented in `README.md` as dependency-free-only.

**Unverified:**

- **The `@AGENTS.md` import in `CLAUDE.md` is not proved to work.** It
  follows the documented Claude Code `@`-import syntax, but nothing in this
  session loaded `AGENTS.md` through it — the rules reached me because you
  dictated them, not because the import fired. Verify with `/memory` in a
  fresh session. `CLAUDE.md` says to read the file manually if the import
  does not expand.
- **Contribution credit.** The noreply alias *should* link commits to your
  profile, but `gh api user/emails` needs the `user:email` scope and this
  token has only `gist, read:org, repo, workflow`. Check the commit avatar
  in a browser.
- **No test suite exists.** The §3 checks are an import and a print, which
  is a low bar honestly labelled as low. `PLAN.md` next step addresses it.
- Not exercised: `uvx`, `uvw.exe` (deprecated shim, left installed).

---

## 2026-10-14 (later) — expand working rules to eight sections

**Model:** unknown — same limitation as the entry above.

**Requested:** add six rules to `AGENTS.md` if missing — read README/PLAN
first and keep decisions in the repo; try the tools before delegating a
technical step; uv-only with recorded versions and commands; meaningful
checks before claiming completion; never weaken a test to make it pass,
explain the disagreement first; never publish secrets, ask before publishing
anything.

**Changed:** `AGENTS.md` 4 sections → 8, 101 → 180 lines. `PLAN.md` Done
list updated, plus a stale `§4` → `§7` pointer fixed after renumbering.
Nothing committed — see Unverified.

**Checked:**

- Compared all six against the existing text before writing. Three were
  already present (§1 read-first, §2 uv-only, §3 verify) and were left
  substantively alone; three were genuinely absent, so this is an addition,
  not a reformat.
- The two halves that were missing from rules I already had: "record
  versions and commands" was not stated in §2, and "keep decisions in the
  repository" had no home at all. Both added to existing sections rather
  than bloating the count.
- §4 names the concrete weakening moves instead of the abstraction: edited
  thresholds, deleted or narrowed cases, `skip`/`xfail`, swallowing the
  exception under test, lowered coverage or tolerance.
- §5 makes the delegation escape hatch auditable — name which capability
  was missing (credentials, admin rights, authorisation, judgment) in one
  clause, so a lazy handoff is checkable.
- §8 promoted from secrets-only to secrets-never-plus-ask-first. The old
  version said nothing about asking before a push.
- `uv run hello.py` and `import hello` still pass after the rewrite.

**Unverified:**

- **Not committed or pushed.** §8 now requires an explicit yes before
  publishing, so the docs sit in the working tree awaiting review. My own
  rule, correctly followed, means I stop here rather than helpfully pushing.
- **§4 is not yet enforceable by anything.** With no test suite there is no
  test to weaken; the rule is intent until `PLAN.md`'s next step lands.
- Standing caveats unchanged: the `@AGENTS.md` import in `CLAUDE.md` is
  still unproven, and the model id still needs `/status`.

**Addendum, same session — things this entry's own rules caught:**

- Renumbering `AGENTS.md` silently broke cross-references in the *other*
  docs: `PLAN.md` pointed at `§4` for a rule that had moved to `§7`.
  Found by grepping all `§` references after the rewrite. Living docs now
  name the rules (`the "verify before you say done" rule`) instead of
  numbering them, which cannot rot the same way; the `§` citations inside
  this file stay, since a run log is a dated record of what was true then.
- `git status` showed `__pycache__/` in the tree — created by my own
  `import hello` check, in a repo whose first rule is "know every file."
  Added `.gitignore` (secrets, caches, venvs, notebooks, run artifacts,
  weights) and confirmed it disappears from `git status --porcelain`.
- Two tool calls failed with `InputValidationError` from malformed JSON
  when the payload contained escaped double-quotes; both were retried in
  other forms rather than reported as blocked, and one retry changed the
  wording of a README line with no loss of meaning.
- Still not pushed, deliberately: `AGENTS.md` §8 requires a yes before
  publishing, and six files are uncommitted awaiting that review.
