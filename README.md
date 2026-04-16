<p align="center">
  <img src="https://getorigin.io/favicon.svg" width="80" alt="Origin Logo" />
</p>

<h1 align="center">Origin</h1>
<p align="center"><strong>git blame for AI-generated code.</strong></p>

<p align="center">
  <a href="https://github.com/dolobanko/origin-cli/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-green" alt="License"></a>
  <a href="https://github.com/dolobanko/origin-cli/stargazers"><img src="https://img.shields.io/github/stars/dolobanko/origin-cli?style=social" alt="GitHub stars"></a>
  <a href="https://getorigin.io"><img src="https://img.shields.io/badge/dashboard-getorigin.io-6366f1" alt="Website"></a>
</p>

```bash
npm i -g https://getorigin.io/cli/origin-cli-latest.tgz
origin init
```

That's it. Origin detects your AI agent, installs hooks, and starts tracking. No accounts, no config files, no server. Everything stays in your git repo.

---

## See it in action

```bash
$ origin blame src/auth.ts
```
```
  1 │ Claude   │ 3h ago  │ import express from 'express';
  2 │ Claude   │ 3h ago  │ import { prisma } from './db';
  3 │ Human    │ 2d ago  │
  4 │ Gemini   │ 1h ago  │ export async function getUsers() {
  5 │ Gemini   │ 1h ago  │   const users = await prisma.user.findMany();
  6 │ Cursor   │ 30m ago │   return users.filter(u => u.active);

  10 lines  Claude: 40%  Gemini: 30%  Cursor: 10%  Human: 20%
```

```bash
$ origin log
```
```
  599d8fc Fix auth bug        — Claude Code · $0.12 · 3 prompts · Apr 14
  def5678 Add rate limiting   — Cursor · $0.08 · 1 prompt · Apr 13
  9ab1234 Update docs         — (no session) · Apr 12

  2/3 commits AI-generated (67%) · $0.20 total cost
```

```bash
$ origin show 599d8fc
```
```
  Session: 02c18ce2 | Claude Code | claude-sonnet-4
  Duration: 14 min | Cost: $0.12 | Tokens: 48,200 | 3 prompts
  Lines: +42 -8

  1. "add rate limiting to prevent brute force"
  2. "also handle the case where user is already locked"
  3. "write tests for both paths"

  Files:
  · src/auth.ts +28 -6
  · src/middleware.ts +14 -2
```

---

## What Origin tracks

Every time an AI agent commits code, Origin captures:

- **Which agent and model** wrote each line (Claude, Cursor, Codex, Gemini, etc.)
- **The prompts** that produced the code — searchable, replayable
- **Cost and tokens** per session, per model, per repo
- **Files changed** with line-level AI vs human attribution

All stored locally in git notes and the `origin-sessions` branch. Your data never leaves your machine unless you push it.

---

## Works with every agent

<table>
<tr>
<td align="center"><img src="https://cdn.simpleicons.org/anthropic/D97757" width="20"><br><sub>Claude Code</sub></td>
<td align="center"><img src="https://cdn.simpleicons.org/cursor/00A4EF" width="20"><br><sub>Cursor</sub></td>
<td align="center"><img src="https://cdn.simpleicons.org/openai/412991" width="20"><br><sub>Codex</sub></td>
<td align="center"><img src="https://cdn.simpleicons.org/google/4285F4" width="20"><br><sub>Gemini CLI</sub></td>
<td align="center">🌊<br><sub>Windsurf</sub></td>
<td align="center">🤖<br><sub>Aider</sub></td>
<td align="center"><img src="https://cdn.simpleicons.org/github/ffffff" width="20"><br><sub>Copilot</sub></td>
<td align="center">⚡<br><sub>Amp</sub></td>
<td align="center">🧩<br><sub>Junie</sub></td>
<td align="center">▶️<br><sub>Continue</sub></td>
<td align="center">🔧<br><sub>Cline</sub></td>
</tr>
</table>

Auto-detected. No per-agent config. One CLI tracks them all.

---

## Daily commands

```bash
# Who wrote this?
origin blame src/auth.ts              # line-level AI/human attribution
origin why src/auth.ts:42             # exact prompt that wrote line 42
origin log                            # git log with session info inline
origin show abc1234                   # full session behind a commit

# What happened?
origin sessions                       # list AI coding sessions
origin search "auth bug"              # find prompt that introduced code
origin diff                           # annotated diff — AI vs human changes
origin stats                          # AI % breakdown by model and repo

# Cross-agent workflow
origin handoff                        # pass context between agents
origin memory show                    # what happened in previous sessions
origin resume                         # pick up where you left off

# Keep it clean
origin backfill                       # retroactively tag old commits
origin audit                          # compliance audit trail
origin report --range 14d             # sprint report with cost breakdown
```

---

## How it works

```
You code with any AI agent
        ↓
Post-commit hook fires automatically
        ↓
Origin detects the agent + reads session data
        ↓
Metadata written to git notes (refs/notes/origin)
Session saved to origin-sessions branch
        ↓
origin blame / log / show / stats read it back
```

Hooks add <50ms to commits. Zero config. Works offline.

### Where data lives

| Location | What's there |
|----------|-------------|
| `refs/notes/origin` | Per-commit metadata — model, session, cost, tokens |
| `origin-sessions` branch | Full session transcripts and prompts |
| `.git/origin-handoff.json` | Cross-agent handoff context |
| `~/.origin/config.json` | CLI config |

Everything travels with `git clone`. No external database.

---

## For teams — [getorigin.io](https://getorigin.io)

The CLI works standalone. The dashboard adds visibility across your team:

- **Session replay** — see every prompt and response in the browser
- **Cost tracking** — who's spending what, on which model, in which repo
- **Policy enforcement** — block AI from touching payment logic, enforce model allowlists
- **PR compliance** — GitHub status checks that verify AI attribution
- **Audit reports** — one-click SOC 2 / ISO 27001 evidence

```bash
origin login                          # connect to your Origin instance
origin init                           # hooks auto-sync with dashboard
```

Free for solo developers. No limits on repos, sessions, or agents.

---

<details>
<summary><strong>Full command reference (50+ commands)</strong></summary>

### Attribution & Analysis
```
origin blame <file>              Line-by-line AI/human attribution
origin diff [range]              Annotated diff with AI attribution
origin stats                     AI vs human stats (--dashboard, --global)
origin compare <a> [b]           Compare attribution between branches
origin prompts <file>            AI prompts that touched a file
origin search <query>            Full-text search across prompts
origin ask <query>               Find the session behind any code
origin rework                    Detect AI code that got reworked
origin backfill                  Retroactive AI tagging for old commits
origin log                       Git log with session info inline
origin show <commit>             Show session linked to a commit
```

### Sessions
```
origin sessions                  List sessions (--all for every repo)
origin session <id>              View session transcript
origin explain [id]              Explain session with prompts and changes
origin export                    Export as CSV / JSON / agent-trace
origin share <id>                Copy session link to clipboard
origin resume                    Resume a previous session
origin rewind                    Rewind to a previous checkpoint
```

### Productivity
```
origin handoff                   Cross-agent context handoff
origin memory                    Session memory across conversations
origin todo                      AI-extracted TODO tracker
origin chat                      Chat with Origin about your codebase
origin snapshot                  Save a point-in-time snapshot
origin analyze                   Deep session pattern analysis
origin recap                     End-of-day summary
```

### Issue Tracker
```
origin issue create <title>      Create an issue (--type, --priority, --dep)
origin issue list                List issues (--status, --priority)
origin issue ready               Next unblocked issue (for AI agent loops)
origin issue close <id>          Close an issue
origin issue dep tree <id>       Show dependency graph
```

### Setup
```
origin init                      Initialize + install hooks
origin login                     Connect to Origin dashboard
origin enable [--global]         Install hooks globally
origin disable                   Remove hooks
origin doctor [--fix]            Diagnose issues
origin upgrade                   Update to latest version
```

### CI/CD & Compliance
```
origin ci check                  AI attribution stats for CI
origin ci session-check          Verify commits have sessions
origin audit                     Compliance audit trail
origin report                    Sprint report (md/json/csv)
origin verify                    Health check
```

</details>

---

## License

MIT
