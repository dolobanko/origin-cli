<p align="center">
  <h1 align="center">Origin CLI</h1>
  <p align="center"><strong>Know exactly what your AI agents are writing.</strong></p>
</p>

<p align="center">
  <a href="https://github.com/dolobanko/origin-cli/blob/main/LICENSE"><img src="https://img.shields.io/badge/license-MIT-green" alt="License"></a>
  <a href="https://github.com/dolobanko/origin-cli/stargazers"><img src="https://img.shields.io/github/stars/dolobanko/origin-cli?style=social" alt="GitHub stars"></a>
</p>

<p align="center">
  Track every AI coding session. Line-level AI/human attribution. Full visibility into AI-authored code.<br/>
  Zero setup — no server, no login, no API keys. All data stored in git.<br/>
  Supports Cursor Agent Trace v0.1.0 standard.
</p>

---

## Install

```bash
npm i -g https://getorigin.io/cli/origin-cli-latest.tgz
```

## Quick Start

```bash
origin init                     # Auto-detects agents, installs hooks globally
# ... code with any AI agent — Origin tracks automatically
origin blame src/index.ts       # See who wrote each line
origin stats                    # AI vs human breakdown
```

That's it. Everything stored locally in git notes and the `origin-sessions` branch.

---

## Top Commands

```bash
origin blame <file>              # Line-by-line AI/human attribution
origin diff [range]              # Annotated diff — see AI changes in context
origin stats                     # AI vs human breakdown for the repo
origin sessions                  # List all AI coding sessions
origin explain [id]              # Full session breakdown: prompts, cost, files
origin search "auth bug"         # Find the prompt that introduced code
origin backfill                  # Retroactively tag old commits as AI/human
origin rework                    # Detect AI code that got reworked by humans
origin report                    # Sprint report — cost, models, ROI
origin audit                     # Compliance audit trail (SOC 2 / ISO 27001)
```

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

---

## All Commands

<details>
<summary><strong>Attribution & Analysis</strong></summary>

```
origin blame <file>             Line-by-line AI/human attribution
  -l, --line <range>              Show specific line range (e.g., 10-20)
  --json                          Output as JSON

origin diff [range]             Annotated diff with AI attribution
  --ai-only                       Only show AI-authored changes
  --human-only                    Only show human-authored changes
  --json                          Output as JSON

origin stats                    AI vs human stats for current repo
  --local                         Compute from local git data (default)
  --dashboard                     Show dashboard stats from API
  -g, --global                    Stats across all repos
  -r, --range <range>             Commit range (e.g., HEAD~50..HEAD)

origin compare <a> [b]          Compare attribution between branches
  --json                          Output as JSON

origin prompts <file>           AI prompts that touched a file
  -e, --expand                    Show code diff for each prompt
  --limit <n>                     Max entries (default: 10)

origin search <query>           Full-text search across prompts
  -l, --limit <n>                 Max results (default: 20)
  --from <date>                   Filter by date (7d, 2w, 1m, ISO date)
  --agent <name>                  Filter by agent
  -m, --model <model>             Filter by model
  -r, --repo <path>               Filter by repo path

origin ask <query>              Find the session behind any code
  -f, --file <path>               Ask about a specific file
  -l, --line <n>                  Focus on a line number
  -s, --session <id>              Search within a session
  --limit <n>                     Max results (default: 5)

origin rework                   Detect reworked AI code
  -d, --days <n>                  Lookback period (default: 7)
  -l, --limit <n>                 Max results (default: 20)

origin backfill                 Retroactive AI tagging
  -d, --days <n>                  How far back (default: 90)
  --dry-run                       Preview only (default)
  --apply                         Actually write git notes
  --min-confidence <level>        high, medium, low (default: medium)

origin analyze                  Analyze AI prompting patterns
  -d, --days <n>                  Days to analyze (default: 30)
  -m, --model <model>             Filter by model
  -e, --export <path>             Export to file
  --json                          Output as JSON

origin verify                   Health check — config, sessions, attribution
  --json                          Output as JSON
```

</details>

<details>
<summary><strong>Sessions & Sharing</strong></summary>

```
origin sessions                 List sessions for current repo
  -s, --status <status>           Filter: unreviewed, approved, rejected, flagged
  -m, --model <model>             Filter by model
  -l, --limit <n>                 Max results (default: 20)
  -a, --all                       Show all repos

origin session <id>             View session detail
origin sessions end <id>        End a running session

origin explain [id]             Explain session: prompts, cost, files, review
  -c, --commit <sha>              Look up by commit SHA
  -s, --short                     Skip prompt-change mapping
  --summarize                     AI-powered summary
  --json                          Output as JSON

origin chat                     Interactive AI assistant for repo context
  -q, --question <text>           Single question (non-interactive)

origin resume [branch]          Resume session from previous branch
  --launch                        Auto-launch AI agent with context
  --json                          Output as JSON

origin rewind                   Time travel to AI checkpoint
  -i, --interactive               Interactive checkpoint browser
  -t, --to <sha>                  Rewind to commit SHA
  --list                          List checkpoints

origin review <id>              Approve/reject/flag a session
  --approve / --reject / --flag
  -n, --note <note>               Review note

origin review-pr <url>          Analyze AI sessions behind a GitHub PR
origin intent-review [branch]   Intent-based review (WHY, not just WHAT)
  -f, --format <format>           json, md (default: terminal)
  -o, --output <file>             Write to file

origin share <id>               Share a session
  -p, --prompt <n>                Share specific prompt only
  -o, --output <path>             Write to file
  --public                        Create public URL (getorigin.io/s/<slug>)

origin export                   Export session data
  -f, --format <format>           json, csv, agent-trace (default: json)
  -o, --output <file>             Write to file
  -l, --limit <n>                 Limit sessions
  -m, --model <name>              Filter by model
  -s, --session <id>              Export specific session
```

</details>

<details>
<summary><strong>Snapshots</strong></summary>

```
origin snapshot                 Save working tree snapshot (no commit)
origin snapshot list            List snapshots for current session
origin snapshot restore <id>    Restore working tree to a snapshot
origin snapshot clean           Remove all snapshot branches
```

</details>

<details>
<summary><strong>Reporting & Compliance</strong></summary>

```
origin report                   Sprint report — cost, models, users, ROI
  -r, --range <range>             7d, 14d, 30d (default: 7d)
  -f, --format <format>           md, json, csv (default: md)
  -o, --output <file>             Write to file

origin audit                    SOC 2 / ISO 27001 compliance audit trail
  --from <date>                   Start date (YYYY-MM-DD)
  --to <date>                     End date (YYYY-MM-DD)
  --author <name>                 Filter by author
  --agent <name>                  Filter by agent
  -f, --format <format>           md, json, csv (default: md)
  -o, --output <file>             Write to file
```

</details>

<details>
<summary><strong>Trail System (Branch Tracking)</strong></summary>

```
origin trail                    Show current trail for this branch
origin trail list               List all trails
  -s, --status <status>           Filter: active, review, done, paused

origin trail create <name>      Create trail for current branch
  -p, --priority <p>              low, medium, high, critical (default: medium)
  -l, --label <labels...>         Labels to add

origin trail update             Update current trail
  -s, --status <status>           active, review, done, paused
  -p, --priority <p>              New priority
  -t, --title <title>             New title

origin trail assign <user>      Assign reviewer
origin trail label <labels...>  Add labels
```

</details>

<details>
<summary><strong>Setup & Configuration</strong></summary>

```
origin login                    Authenticate with Origin platform
origin init                     Initialize + install global hooks
  --standalone                    Force standalone mode

origin enable                   Install hooks
  -a, --agent <agent>             claude-code, cursor, gemini, windsurf, codex, aider
  -g, --global                    Install globally for all repos
  -l, --link <slug>               Link repo to agent
  --no-chain                      Replace hooks instead of chaining

origin disable                  Remove hooks
  -g, --global                    Remove global hooks

origin link [slug]              Link repo to agent (.origin.json)
  --clear                         Remove mapping

origin status                   Show system status
origin whoami                   Show user/org info
origin upgrade                  Upgrade CLI to latest
  --check                         Only check, don't install
```

</details>

<details>
<summary><strong>Configuration Keys</strong></summary>

```
origin config get <key>         Get a config value
origin config set <key> <value> Set a config value
origin config list              List all config values
```

| Key | Type | Values | Description |
|-----|------|--------|-------------|
| `apiUrl` | string | | Origin API URL |
| `apiKey` | string | | API key |
| `orgId` | string | | Organization ID |
| `userId` | string | | User ID |
| `machineId` | string | | Machine identifier |
| `commitLinking` | enum | `always`, `prompt`, `never` | When to add Origin-Session trailers to commits |
| `pushStrategy` | enum | `auto`, `prompt`, `false` | When to push origin-sessions branch |
| `telemetry` | boolean | | Anonymous telemetry (opt-in) |
| `autoUpdate` | boolean | | Check for CLI updates |
| `secretRedaction` | boolean | | Redact secrets before API send |
| `hookChaining` | boolean | | Chain existing hooks |
| `checkpointRepo` | string | | External git remote for session data |
| `mode` | enum | `auto`, `standalone` | Force standalone mode (skip API even when logged in) |

</details>

<details>
<summary><strong>Database & CI/CD</strong></summary>

```
origin db import                Import prompts into local DB
  -f, --format <format>           origin-sessions (default) or agent-trace
  --file <path>                   Input file (agent-trace format)

origin db stats                 Show local database statistics

origin ci check                 AI attribution report for CI
  -r, --range <range>             Commit range

origin ci squash-merge <base>   Preserve attribution through squash merge
origin ci generate-workflow     Generate GitHub Actions YAML
```

</details>

<details>
<summary><strong>Ignore, Plugins & Proxy</strong></summary>

```
origin ignore                   List ignore patterns
origin ignore add <pattern>     Add pattern to .origin.json
origin ignore remove <pattern>  Remove pattern
origin ignore test <filepath>   Test if file would be ignored

origin plugin list              List installed plugins
origin plugin install <n> <cmd> Install plugin
origin plugin remove <name>     Remove plugin

origin proxy install            Install git proxy for attribution preservation
origin proxy uninstall          Remove git proxy
origin proxy status             Show proxy status
```

</details>

<details>
<summary><strong>Maintenance</strong></summary>

```
origin doctor                   Diagnose stuck/orphaned sessions
  -f, --fix                       Auto-fix issues
  -v, --verbose                   Detailed diagnostics

origin reset                    Clear local session state
  -f, --force                     Force clear

origin clean                    Remove orphaned data
  --dry-run                       Preview (default)
  -f, --force                     Actually delete

origin web                      Local web dashboard
  -p, --port <n>                  Port (default: 3141)
```

</details>

<details>
<summary><strong>Team & Platform (Connected Mode)</strong></summary>

```
origin repos                    List repositories
origin repo:add                 Add repository (--name, --path, --provider)
origin sync                     Sync session data from repo
origin agents                   List agents
origin agent:create             Create agent (--name, --slug, --model)
origin policies                 List governance policies
origin policy:versions <id>     Policy version history
origin agent:versions <id>      Agent version history
origin team                     List team members
origin user <id>                View user detail
origin notifications            View notifications (--unread, --limit)
```

</details>

---

## Usage Examples

### Who wrote this code?

```bash
origin blame src/api.ts
```
```
  1 | Claude   | 3h ago  | import express from 'express';
  2 | Claude   | 3h ago  | import { prisma } from './db';
  3 | Human    | 2d ago  |
  4 | Gemini   | 1h ago  | export async function getUsers() {
  5 | Gemini   | 1h ago  |   const users = await prisma.user.findMany();
  6 | Cursor   | 30m ago |   return users.filter(u => u.active);

10 lines  Claude: 40%  Gemini: 30%  Cursor: 10%  Human: 20%
```

### Retroactive attribution

```bash
origin backfill                      # Dry-run — shows what it would tag
origin backfill --apply              # Actually write the tags
origin backfill --days 180           # Go back 6 months
origin backfill --min-confidence high
```

### Find the prompt behind any code

```bash
origin search "authentication"
origin search "refactor" --agent cursor --from 7d
origin ask "who wrote the auth middleware" --file src/auth.ts
```

### Sprint report

```bash
origin report --range 14d --format json --output sprint.json
```

### Compliance audit

```bash
origin audit --from 2026-01-01 --to 2026-03-31 --format csv --output q1.csv
```

### Intent review for PRs

```bash
origin intent-review feature/auth --format md --output review.md
```

### Export for external tools

```bash
origin export --format agent-trace --session abc123  # Cursor Agent Trace v0.1.0
origin export --format csv --output sessions.csv     # Spreadsheet
```

---

## Features

### AI Attribution Context

Origin automatically injects context into AI agent system prompts so agents know what other agents have already done.

**Repo-level** (session start):
```
Repository AI context: 90% of recent commits (27/30) are AI-generated.
  - claude-code wrote src/api.ts, src/hooks.ts on 2026-03-22
  - gemini-cli wrote src/utils.ts on 2026-03-21
```

**Per-file** (when an agent reads/edits a file):
```
File attribution for src/hooks.ts: 95% AI-generated (2258/2388 lines).
  Lines 1-28: claude-code (claude-opus-4-6)
  Lines 217-240: human (KIRAN)
```

### Secret Scanner

Pre-commit hook blocks commits containing hardcoded secrets:

```
  AWS Access Key     config.env:3   AKIA****MPLE
  GitHub Token       src/api.ts:12  ghp_****ab12
  2 secrets found. Commit blocked.
```

Detects: AWS keys, GitHub/GitLab tokens, OpenAI/Anthropic/Stripe keys, JWTs, database connection strings, private keys, and `*_TOKEN=`/`*_SECRET=`/`*_KEY=` patterns.

### Governance Policies (Connected Mode)

Define rules for AI agent usage across your organization:
- Cost limits per session/model/day
- Allowed models and agents
- Required review for sessions above thresholds
- File access restrictions

---

## How It Works

```
AI Agent commits code -> Post-commit hook fires -> Origin detects AI process
-> Writes git note (model, session, cost) -> Writes session to origin-sessions branch
-> origin blame / stats / diff read notes for attribution
```

### Data Storage

| Location | Purpose |
|----------|---------|
| `refs/notes/origin` | Per-commit AI metadata (model, session, cost, tokens) |
| `origin-sessions` branch | Session transcripts, prompts, file changes |
| `~/.origin/config.json` | CLI config |
| `~/.origin/agent.json` | Agent config (machine ID, detected tools) |
| `~/.origin/git-hooks/` | Global hook scripts |
| `~/.origin/plugins.json` | Installed plugins |
| `~/.origin/db/` | Local SQLite prompt database |
| `.origin.json` | Per-repo config (agent link, ignore patterns) |

---

## Origin vs Alternatives

| Feature | Origin | git-ai | Entire.io |
|---------|--------|--------|-----------|
| Line-level attribution | **Yes** -- per-line AI/human tags | Commit-level only | No |
| Retroactive tagging | **Yes** -- `origin backfill` | No | No |
| Local-first / no server | **Yes** -- git notes, zero setup | Yes | No -- SaaS only |
| Multi-agent support | **6 agents** | Claude only | GitHub Copilot only |
| Session transcripts | **Full prompts + responses** | No | No |
| Per-file context injection | **Yes** -- agents see authorship | No | No |
| Secret scanning | **Built-in** pre-commit hook | No | No |
| Intent-based review | **Yes** -- `origin intent-review` | No | No |
| Plugin system | **Yes** -- JSON stdin/stdout protocol | No | No |
| CI/CD integration | **Yes** -- `origin ci check` | No | No |
| Open source | **MIT** | MIT | Closed |

---

## For Teams

**[getorigin.io](https://getorigin.io)** -- centralized dashboard, policy enforcement, PR compliance. Free trial.

Connected mode adds: real-time dashboard, budget controls with ROI calculator, weekly digest emails, model/cost policies, PR blocking, compliance reports, IAM with per-user API keys, team leaderboards, Slack notifications, and GitHub App integration.

```bash
origin login    # Authenticate with your Origin instance
origin init     # Register machine + install hooks
```

---

## License

MIT
