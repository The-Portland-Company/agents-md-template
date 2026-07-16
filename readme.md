# agents-md-template

A drop-in starter for agent operating instructions, split into two layers so multiple AI agents (and humans) can work the same repo without stepping on each other.

## Files

| File | Scope | Read order |
| --- | --- | --- |
| **`Agents.md`** | Client-agnostic operating manual — values, communication, autonomy, deploy gating, security, Supabase/Railway, git & CI/CD, testing, memory, monitoring. Any runtime reads this. | First |
| **`Claude.md`** | Claude Code–specific mechanics only (Skills, `/focus-forge` Tasks, HITL question format, notifications, subagents, memory dir). References `Agents.md` at the top. | After `Agents.md` |

The split keeps one source of truth: shared principles live in `Agents.md`; only runtime-specific wiring lives in `Claude.md`. Add sibling files (e.g. a Codex or OpenClaw layer) the same way.

## How to use

1. Copy `Agents.md` and `Claude.md` into your repo root.
2. Delete or adapt any stack sections that don't apply — rules bind only when their stack (Supabase, Railway, `.env`, logs) is actually present (see **Scope & Precedence** in `Agents.md`).
3. Set your deploy branch name and secrets/notification config where noted.

## Secret scanning (GitLeaks)

A version-controlled pre-commit hook scans staged changes for secrets with [GitLeaks](https://github.com/gitleaks/gitleaks) and blocks the commit if any are found. Enable it once per clone:

```bash
brew install gitleaks        # if not already installed
git config core.hooksPath .githooks
```

Rules and allowlists live in `.gitleaks.toml`. Override a scan in a genuine false-positive with `git commit --no-verify`.

## Core coordination principles

- **Respect pre-commit hooks and CI/CD** — never bypass without explicit human authorization.
- **PR + auto-merge on deploy** — never push straight to the deploy branch, so agents and humans don't overwrite each other.
- **Claim work as Tasks** (`/focus-forge` in Claude Code) and post progress comments, especially on blockers.
