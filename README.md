<p align="center">
  <h1 align="center">Origin CLI</h1>
  <p align="center"><strong>Know exactly what your AI agents are writing.</strong></p>
</p>

<p align="center">
  <a href="https://www.npmjs.com/package/@anthropic/origin-cli"><img src="https://img.shields.io/npm/v/@anthropic/origin-cli?color=blue&label=npm" alt="npm version"></a>
  <a href="https://github.com/dolobanko/origin-cli/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-green" alt="License"></a>
  <a href="https://github.com/dolobanko/origin-cli/stargazers"><img src="https://img.shields.io/github/stars/dolobanko/origin-cli?style=social" alt="GitHub stars"></a>
</p>

<p align="center">
  Track every AI coding session. Line-level AI/human attribution. Full visibility into AI-authored code.<br/>
  Zero setup — no server, no login, no API keys. All data stored in git.
</p>

<!-- TODO: Add screenshot of `origin blame` output -->
<!-- <p align="center"><img src="docs/assets/origin-blame.png" width="700" alt="origin blame output"></p> -->

---

## 5 commands you'll use every day

```bash
origin blame src/index.ts   # Who wrote each line — AI or human?
origin diff                  # Annotated diff with AI attribution
origin stats                 # AI vs human breakdown for the repo
origin sessions              # List all AI coding sessions
origin web                   # Full dashboard in the browser
```

<!-- TODO: Add 30-second GIF demo: origin init → commit → origin blame -->
<!-- <p align="center"><img src="docs/assets/demo.gif" width="700" alt="Origin demo"></p> -->

---

## Install

```bash
# npm (recommended)
npm i -g @anthropic/origin-cli

# or direct
npm i -g https://getorigin.io/cli/origin-cli-latest.tgz
```

## Quick Start

```bash
origin init    # Auto-detects AI agents, installs hooks
# ... code with any AI agent — Origin tracks automatically
origin blame src/index.ts
```

That's it. Everything stored locally in git notes and the `origin-sessions` branch.

---

## Supported Agents

| Agent | Hook Type | Status |
|-------|-----------|--------|
| <img src="https://cdn.simpleicons.org/anthropic/D97757" width="14"> **Claude Code** | Session hooks + process detection | **Supported** |
| <img src="https://cdn.simpleicons.org/cursor/00A4EF" width="14"> **Cursor** | Session hooks + Cursor DB | **Supported** |
| <img src="https://cdn.simpleicons.org/openai/412991" width="14"> **Codex CLI** | Session hooks + process detection | **Supported** |
| <img src="https://cdn.simpleicons.org/google/4285F4" width="14"> **Gemini CLI** | Session hooks + process detection | **Supported** |
| Windsurf | Session hooks | Coming soon |
| Aider | Config hooks | Coming soon |
| GitHub Copilot | Process detection | Coming soon |

---

## Commands

<details>
<summary><strong>Attribution</strong> — see what AI wrote</summary>

```
origin blame <file>             AI/human attribution per line ([AI]/[HU] tags)
origin diff [range]             Annotated diff with AI/human attribution
origin stats                    AI vs human stats (--dashboard, --global)
origin compare <a> [b]          Compare AI attribution between branches
origin prompts <file>           AI prompts that touched a file (--expand for diffs)
origin search <query>           Search AI prompt history
origin ask <query>              Which AI session wrote specific code
origin chat                     Interactive AI assistant (needs ANTHROPIC_API_KEY)
origin web                      Local web dashboard
origin analyze                  Prompt pattern analytics
```

</details>

<details>
<summary><strong>Sessions</strong> — track AI activity</summary>

```
origin sessions                 List sessions for current repo (--status, --model, --all)
origin sessions end <id>        End a running session
origin session <id>             View session with full transcript
origin explain [id]             Explain session with prompts and changes
origin export                   Export session data as CSV/JSON
origin resume [branch]          Resume session context for AI handoff
origin share <id>               Share session (clipboard or --public URL)
origin share <id> --public      Create public link: getorigin.io/s/<slug>
```

</details>

<details>
<summary><strong>Setup & Config</strong></summary>

```
origin init                     Initialize + install global hooks
origin enable [--global]        Install hooks + secret scanner
origin disable [--global]       Remove hooks
origin status                   Show system status
origin upgrade                  Upgrade CLI to latest version
origin config set <key> <val>   Set CLI config
origin config get <key>         Get config value
origin config list              List all config
origin ignore                   Manage ignore patterns
```

</details>

<details>
<summary><strong>Maintenance & Time Travel</strong></summary>

```
origin verify [--json]          Health check — agents, repo, sessions
origin doctor [--fix]           Diagnose and fix issues
origin clean [--force]          Remove orphaned data
origin rewind                   Rewind to previous AI checkpoint
origin trail                    Branch-centric work tracking
```

</details>

---

## Features

### AI Attribution Context

Origin automatically injects context into AI agent system prompts so agents know what other agents have already done.

**Repo-level** (session start) — agents see a summary of recent AI activity:
```
Repository AI context: 90% of recent commits (27/30) are AI-generated.
  - claude-code wrote src/api.ts, src/hooks.ts on 2026-03-22
  - gemini-cli wrote src/utils.ts on 2026-03-21
Top AI-modified files: src/commands/hooks.ts (8 AI commits)
```

**Per-file** (pre-tool-use) — when an agent reads/edits a file, it gets line-level authorship:
```
File attribution for src/hooks.ts: 95% AI-generated (2258/2388 lines).
  Lines 1-28: claude-code (claude-opus-4-6)
  Lines 217-240: human (KIRAN)
  Lines 1508-1794: claude-code (claude-opus-4-6)
```

Both features are automatic. Data comes from git notes and git blame.

### Secret Scanner

Pre-commit hook blocks commits containing hardcoded secrets:

```
  AWS Access Key     config.env:3   AKIA****MPLE
  GitHub Token       src/api.ts:12  ghp_****ab12
  2 secrets found. Commit blocked.
```

Detects: AWS keys, GitHub/GitLab tokens, OpenAI/Anthropic/Stripe keys, Slack tokens, JWTs, database connection strings, private keys, and `*_TOKEN=`/`*_SECRET=`/`*_KEY=` patterns.

### AI Chat

```bash
export ANTHROPIC_API_KEY=<your-key>   # Add to ~/.zshrc
origin chat
```

```
  you > who wrote the auth module?
  Claude 3.5 Sonnet wrote src/auth.ts across 3 sessions...

  you > how much have I spent on AI this month?
  $4.32 across 23 sessions. Claude: $3.18 (74%), Gemini: $1.14 (26%)
```

Single question mode: `origin chat -q "what did AI touch last week?"`

---

## How It Works

```
AI Agent commits code → Post-commit hook fires → Origin detects AI process
→ Writes git note (model, session, cost) → Writes session to origin-sessions branch
→ origin blame / stats / diff read notes for attribution
```

Pre-commit hook scans for secrets and blocks if found.

### Data Storage

| Location | Purpose |
|----------|---------|
| `refs/notes/origin` | Per-commit AI metadata (model, session, cost, tokens) |
| `origin-sessions` branch | Session transcripts, prompts, file changes |
| `~/.origin/config.json` | CLI config |
| `~/.origin/git-hooks/` | Global hook scripts |

---

## Origin vs Alternatives

| Feature | Origin | git-ai | Entire.io |
|---------|--------|--------|-----------|
| Line-level attribution | **Yes** — per-line AI/human tags | Commit-level only | No |
| Local-first / no server | **Yes** — git notes, zero setup | Yes | No — SaaS only |
| Multi-agent support | **4 agents**, more coming | Claude only | GitHub Copilot only |
| Session transcripts | **Full prompts + responses** | No | No |
| Per-file context injection | **Yes** — agents see authorship | No | No |
| Secret scanning | **Built-in** pre-commit hook | No | No |
| Open source | **MIT** | MIT | Closed |

---

## For Teams

**[getorigin.io](https://getorigin.io)** — centralized dashboard, policy enforcement, PR compliance. Free trial.

Connected mode adds: real-time dashboard, model/cost policies, PR blocking (`[AI 73%]` annotations on GitHub PRs), compliance reports, team leaderboards, Slack notifications, and GitHub App integration.

```bash
origin login    # Authenticate with your Origin instance
origin init     # Register machine + install hooks
```

---

## License

MIT
