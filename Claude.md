# Claude.md — Claude Code Specifics

> **Read `Agents.md` first.** It holds the client-agnostic operating manual (values, communication, autonomy, deploy gating, security, Supabase/Railway, git & CI/CD, testing, memory, monitoring). This file adds **only** the Claude Code–specific mechanics on top of it. If a rule isn't Claude-specific, it lives in `Agents.md`.

## Skills

- Prefer a matching **Skill** over ad-hoc work. Invoke via the Skill tool; when the human types `/<skill-name>`, run that skill. Only use skills that are actually listed — don't guess names, and if none fits, do the work directly.
- Screenshots: **playwright skill only** (no remote browser MCP). Immediately `Read` the image so it renders inline.
- Domain runbooks live under `~/.claude/agent-docs/` — read one only when that topic is in scope (GitHub, Railway, Cloudflare, Supabase, test email/OTP, Forge, App Store). Never `railway login` / `wrangler login` / `gcloud auth login` yourself — follow the runbook.

## Tasks — Focus Forge (coordination)

- **Always create Tasks in Claude and `/focus-forge` when working on Plans, Goals, or general work that involves them.** This claims the work so other Agents and humans don't act on the same item at once, and lets them monitor progress.
  - Create the Task before starting; keep its status current (`in_progress` → `completed`).
  - **Post Comments with your progress** — especially whenever you hit an issue that delays you, so blockers are visible to other Agents and humans.
- When told "Add these tasks" / "create a Task" → create them in **`/focus-forge`**, not a `todo.md`. Do not maintain checkbox task lists in Markdown.
- "Task: Do X" → spawn subagent(s) and do X.

## HITL Questions (only when truly blocked)

Use the blocking format only for real blockers (missing secret you can't retrieve, irreversible risk with no authority, true fork with no precedent):

```
===
1. **__Every line bold + underlined like this?__**
2. **__Second ask only if independently blocking?__**
===
```

- Bold+underline **every** numbered line (`**__…__**`). Continuous numbering `1…n`. Minimal prose before the block.
- Never append `(yes/no)` — it's implied. Never pad with "I'll do it in seconds." Prefer one ask.

## Notifications

- "Notify me when done" → actually send an SMS to the configured number via the configured provider (don't just say you will). If no number/provider is configured, say you can't notify — don't pretend to send.

## Subagents & Verification

- Spawn subagents for independent or parallel work; send concurrent agents in a single message so they run at once.
- If the runtime exposes agent terminals, close finished agents' tabs; never kill a live agent window.
- Shut down a server → restart it unless told shutdown-only.
- Claimed monitoring → spawn a real watcher the same turn (`Monitor`, `/loop`, background command + polling), or say you can't.

## Memory (Claude Code)

- Persistent memory lives under `~/.claude/…/memory/`. One fact per file with frontmatter; add a one-line pointer to `MEMORY.md` (the index loaded each session — never put memory content there).
- Convert relative dates to absolute. Don't save what the repo already records. Update the existing file rather than duplicating; delete memories that turn out wrong.

## Output Conventions

- Comparisons → table with ✅/❌/⚠️, minimal prose.
- Reference code as `file_path:line_number` (clickable).
