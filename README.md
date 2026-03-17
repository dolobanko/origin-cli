# Origin CLI

**Know exactly what your AI agents are writing.**

Origin CLI tracks every AI coding session, provides line-level AI/human attribution, and gives you full visibility into AI-authored code. Works locally with zero setup — no server, no login, no API keys. All data stored in git.

For teams needing dashboard, policy enforcement, and compliance features — [Origin Connected](https://getorigin.io) is available as a managed service.

---

## Install

```bash
npm i -g https://getorigin.io/cli/origin-cli-latest.tgz
```

## Quick Start

```bash
# 1. Initialize (auto-detects installed AI agents, installs global hooks)
origin init

# 2. Code with any AI agent — Origin tracks automatically

# 3. See what AI wrote
origin blame src/index.ts    # Line-level AI/human attribution
origin stats                 # AI vs human breakdown
origin diff                  # Annotated diff with attribution
origin sessions              # List all AI sessions
origin session <id>          # Full session transcript with prompts
origin prompts src/index.ts  # AI prompts that touched this file
origin web                   # Local dashboard in the browser
origin chat                  # Ask questions in natural language (requires ANTHROPIC_API_KEY)
```

That's it. Everything stored locally in git notes and the `origin-sessions` branch.

---

## Commands

```
Setup:
  origin init                     Initialize agent + install global hooks
  origin enable [--global]        Install hooks (all repos or current repo)
  origin disable [--global]       Remove hooks
  origin status                   Show system status
  origin upgrade                  Upgrade CLI to latest version

Attribution:
  origin blame <file>             AI/human attribution per line ([AI]/[HU] tags)
  origin diff [range]             Annotated diff with AI/human attribution
  origin stats                    AI vs human commit/line breakdown with charts
  origin search <query>           Search AI prompt history
  origin ask <query>              Query which AI session wrote specific code
  origin prompts <file>           Show AI prompts that touched a file (--expand for diffs)
  origin chat                     Interactive AI assistant — natural language questions
  origin web                      Local web dashboard in the browser
  origin analyze                  Prompt pattern analytics

Sessions:
  origin sessions                 List sessions (--status, --model, --limit)
  origin session <id>             View session detail with full transcript
  origin resume [branch]          Resume session context for AI handoff
  origin explain [id]             Explain session with prompts and changes
  origin share <id>               Create shareable session bundle

Time Travel:
  origin rewind                   Rewind to previous AI checkpoint
  origin trail                    Branch-centric work tracking

Maintenance:
  origin doctor [--fix]           Diagnose and fix issues
  origin clean [--force]          Remove orphaned data
```

---

## Features

- **AI Blame** — `[AI]`/`[HU]` tag per line with model name and git author
- **AI Diff** — annotated diffs with AI/human attribution per line
- **Stats** — AI vs human commit/line breakdown with tool and model charts
- **Ask the Author** — find which AI session and prompt generated specific code
- **Prompt History** — every AI prompt that touched a file, with diffs
- **AI Chat** — interactive assistant for natural language questions about your AI code
- **Web Dashboard** — local browser UI with stats, commits, sessions, and prompts
- **Session Tracking** — full transcript of every AI session stored in git
- **Session Resume** — rebuild context from previous sessions for handoff between agents
- **Search** — search all AI prompt history locally
- **Prompt Analytics** — detect prompting patterns and metrics
- **Trail System** — branch-centric work tracking linking sessions to features
- **Attribution Preservation** — AI tags survive rebase, amend, cherry-pick, and stash
- **Auto-Detection** — 13 agents detected automatically
- **Git Notes** — per-commit AI metadata stored in `refs/notes/origin`

---

## AI Chat Setup

`origin chat` uses the Anthropic API to answer natural language questions about your AI-authored code:

```bash
export ANTHROPIC_API_KEY=sk-ant-...    # Add to ~/.zshrc or ~/.bashrc
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
AI Agent commits code
        ↓
Global post-commit hook fires
        ↓
Origin detects AI process (pgrep) or active session
        ↓
Writes git note to refs/notes/origin with model, session, cost
        ↓
Writes session data to origin-sessions branch
        ↓
origin blame / stats / diff / web read notes for attribution
```

### Supported Agents

| Agent | Detection | Status |
|-------|-----------|--------|
| Claude Code | Session hooks + process detection | Stable |
| Gemini CLI | Process detection | Stable |
| Cursor | Session hooks | Stable |
| Codex CLI | Session hooks + process detection | Stable |
| Aider | Process detection | Stable |
| Windsurf | Session hooks + process detection | Preview |
| GitHub Copilot | Process detection | Preview |
| Continue | Process detection | Preview |
| Amp | Process detection | Preview |
| Junie | Process detection | Preview |
| OpenCode | Process detection | Preview |
| Rovo Dev | Process detection | Preview |
| Droid | Process detection | Preview |

### Data Storage

| Location | Purpose |
|----------|---------|
| `refs/notes/origin` | Per-commit AI metadata (model, session, cost, tokens) |
| `origin-sessions` branch | Session transcripts, prompts, file changes |
| `~/.origin/config.json` | CLI config |
| `~/.origin/git-hooks/` | Global hook scripts |
| `~/.origin/db/prompts.json` | Local prompt database for search |

---

## Connected Mode

For teams needing centralized governance, connect to [Origin Platform](https://getorigin.io):

```bash
origin login    # Authenticate with your Origin instance
origin init     # Register machine + install hooks
```

Connected mode adds: dashboard, policy enforcement, PR blocking, compliance reports, team leaderboards, Slack notifications, and GitHub App integration.

---

## License

MIT
