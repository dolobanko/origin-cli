# Origin CLI

Open-source tool for tracking, attributing, and governing AI-assisted code. Works **standalone** (no server) or **connected** to the [Origin platform](https://getorigin.io).

Hooks into AI coding agents — Claude Code, Cursor, Gemini CLI, Windsurf, Aider, GitHub Copilot — to capture sessions, provide attribution, and optionally enforce policies.

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

## Two Modes

### 🟢 Standalone (no server needed)
Everything runs locally, stored in git. No account, no API key, no server.

- Session tracking & history
- AI attribution & `origin blame`
- Code search & analysis
- Time travel & session resume
- Local stats dashboard

### 🔵 Connected (Origin platform)
Everything above **plus** team governance:

- Policy enforcement (block file access, cost limits)
- Team dashboards & analytics
- Session reviews (approve/reject/flag)
- Compliance audit log
- PR gating & CI/CD integration

## Install

```bash
npm i -g https://getorigin.io/cli/origin-cli-latest.tgz
```

Requires Node.js 18+. Verify: `origin --version`

## Quick Start

### Standalone (2 commands)

```bash
cd your-project
origin init       # detect tools, install hooks — no login needed
```

Start coding with any supported AI agent. Sessions are tracked automatically.

```bash
origin sessions           # list local sessions
origin blame src/app.ts   # see AI vs human attribution
origin stats              # local analytics
origin search "auth"      # search AI-generated code
```

### Connected (3 commands)

```bash
origin login              # authenticate with Origin platform
origin init               # register machine + install hooks
```

Now you get everything above plus policies, reviews, and dashboards.

```bash
origin policies           # view active policies
origin review <id>        # review a session
origin audit              # compliance audit log
```

## Commands

### Works in Both Modes

| Command | Description |
|---------|-------------|
| `origin init` | Detect AI tools, install hooks, configure project |
| `origin sessions` | List coding sessions (local git or API) |
| `origin session <id>` | View session details, transcript, changes |
| `origin blame <file>` | Line-by-line AI vs human attribution |
| `origin stats` | Dashboard — sessions, tokens, costs, models |
| `origin search <query>` | Search across AI-generated code changes |
| `origin diff <id>` | View code changes from a session |
| `origin trail` | View decision trail for current branch |
| `origin resume <id>` | Resume a previous session with context |

### Connected Mode Only (requires `origin login`)

| Command | Description |
|---------|-------------|
| `origin login` | Authenticate with Origin platform |
| `origin policies` | View & manage enforcement policies |
| `origin review <id>` | Approve, reject, or flag a session |
| `origin audit` | View compliance audit log |
| `origin agents` | List registered AI agents |
| `origin repos` | List tracked repositories |
| `origin sync` | Force sync local data to platform |

Running a connected-only command without login shows:
```
  'origin policies' requires the Origin platform.
  Run: origin login
```

## How It Works

Origin installs hooks into your AI coding tools. These fire on session lifecycle events:

```
session-start       → Create session, capture context
user-prompt-submit  → Record prompts, track conversation
pre-tool-use        → Enforce policies (block restricted files)
post-tool-use       → Track branch changes, file modifications
stop / session-end  → Finalize session, write git metadata
```

### Standalone Mode
- Session ID: `local-<uuid>` (generated locally)
- State: `.git/origin-session-*.json`
- History: `origin-sessions` git branch
- Attribution: `refs/notes/origin` git notes
- No network calls

### Connected Mode
- Session ID: issued by Origin API
- Real-time sync to platform
- Policy enforcement from `pre-tool-use` hook
- Heartbeat monitoring

## Supported Agents

| Agent | Auto-detected | Hook Method |
|-------|--------------|-------------|
| Claude Code | ✅ | `.claude/hooks/` config |
| Cursor | ✅ | Rules file |
| Gemini CLI | ✅ | Hook config |
| Windsurf | ✅ | Rules file |
| Aider | ✅ | Git hooks |
| GitHub Copilot | ✅ | Git hooks |

`origin init` detects installed tools and configures hooks automatically.

## Feature Comparison: Standalone vs Connected

| Feature | Standalone | Connected |
|---------|:----------:|:---------:|
| Session tracking | ✅ | ✅ |
| AI attribution & blame | ✅ | ✅ |
| Code search | ✅ | ✅ |
| Local stats | ✅ | ✅ |
| Time travel & resume | ✅ | ✅ |
| Trail system | ✅ | ✅ |
| Policy enforcement | — | ✅ |
| Team dashboards | — | ✅ |
| Session reviews | — | ✅ |
| Audit log | — | ✅ |
| PR gating | — | ✅ |

## Origin vs Entire vs git-ai

|  | **Origin CLI** | **[Entire](https://entire.io)** | **[git-ai](https://usegitai.com)** |
|--|:-:|:-:|:-:|
| **Core approach** | Session lifecycle hooks | Git hooks + shadow branches | `git-ai checkpoint` + Git notes |
| **What it captures** | Full session: prompts, responses, tool calls, costs | Session transcripts + checkpoints | Code attribution only (line → agent) |
| | | | |
| **Session & Tracking** | | | |
| Session capture | ✅ Full lifecycle (6 hook events) | ✅ Transcript capture on push | ❌ |
| Session history | ✅ `origin sessions` | ✅ Web dashboard | ❌ |
| Session explain | ✅ `origin explain --summarize` | ✅ `entire explain` (AI summary) | ❌ |
| Ask about code | ✅ `origin ask` (file/session/query) | ❌ | ✅ `/ask` (query author) |
| Session sharing | ✅ `origin share` (bundle/clipboard) | ❌ | ❌ |
| Session resume | ✅ `origin resume` (restore context) | ❌ | ❌ |
| | | | |
| **Attribution & Blame** | | | |
| AI blame | ✅ `origin blame` (line-level) | ❌ | ✅ `git-ai blame` |
| Attributed diffs | ✅ `origin diff` (AI/human annotations) | ❌ | ❌ |
| Time travel / rewind | ✅ `origin rewind` (interactive browser) | ✅ `entire rewind` | ❌ |
| | | | |
| **Search & Analysis** | | | |
| Code search | ✅ `origin search` (across all prompts) | ❌ | ❌ |
| Analytics | ✅ `origin stats` (tokens, costs, models) | ❌ | ✅ `git-ai stats` |
| Pattern analysis | ✅ `origin analyze` (prompting patterns, trends) | ❌ | ❌ |
| | | | |
| **Governance** | | | |
| Policy enforcement | ✅ Real-time blocking (pre-tool-use) | ❌ | ❌ |
| Session reviews | ✅ Approve/reject/flag | ❌ | ❌ |
| Audit log | ✅ SOC 2 ready | ❌ | ❌ |
| PR gating | ✅ Block unreviewed PRs | ❌ | ❌ |
| Team dashboards | ✅ Connected mode | ✅ Web dashboard | ✅ Enterprise |
| | | | |
| **DevOps & Workflow** | | | |
| CI/CD integration | ✅ `origin ci` (check, squash-merge, GitHub Actions) | ❌ | ❌ |
| Trail system | ✅ `origin trail` (branch-scoped work tracking) | ❌ | ❌ |
| Plugin system | ✅ `origin plugin` (install custom commands) | ❌ | ❌ |
| Git proxy | ✅ `origin proxy` (transparent attribution) | ❌ | ❌ |
| Local database | ✅ `origin db` (import, stats, manage) | ❌ | ❌ |
| Diagnostics | ✅ `origin doctor` + `origin clean` | ❌ | ❌ |
| | | | |
| **Platform** | | | |
| IDE integration | ❌ | ❌ | ✅ VS Code gutter annotations |
| Works offline | ✅ | ✅ | ✅ |
| Zero config | ✅ `origin init` | ✅ `entire enable` | ✅ Auto |
| Agents supported | 6 (Claude Code, Cursor, Gemini, Windsurf, Aider, Copilot) | 5 (Claude Code, Gemini, OpenCode, Cursor, Copilot) | 12+ |
| Language | TypeScript (Node.js) | Go | Go |
| License | MIT | Proprietary | Apache 2.0 |
| | | | |
| **Feature count** | **35+ commands** | **~5 commands** | **~6 commands** |

### Why Origin

**35+ commands vs 5-6.** Origin isn't just a tracker — it's a full governance platform. Session lifecycle, attribution, search, analysis, trails, CI/CD, plugins, diagnostics. Entire and git-ai solve one problem each; Origin solves the whole workflow.

**Session governance, not just tracking.** Origin captures the complete session lifecycle and lets you enforce policies *during* the session — blocking restricted file access, enforcing cost limits, requiring reviews before merge. Entire and git-ai track what happened; Origin controls what's allowed to happen.

**Standalone-first.** Works fully offline with zero setup. Add the platform later for team features — nothing breaks, nothing migrates.

### When to use Entire

Entire is a good fit if you want a **visual dashboard** for browsing AI coding sessions and checkpoints, with AI-powered session summaries. It captures transcripts and supports rewind to previous checkpoints. No policy enforcement, attribution, or analysis features.

### When to use git-ai

git-ai is the best choice if you only need **line-level attribution** — knowing which AI agent wrote each line. Broadest agent support (12+), VS Code integration with gutter annotations, and the `/ask` command to query the original AI about its code. No session tracking, governance, or analysis.

## Configuration

Config file: `~/.origin/config.json`

```json
{
  "serverUrl": "https://getorigin.io",
  "apiKey": "org_...",
  "machineId": "machine_..."
}
```

No config file = standalone mode. Add `apiKey` via `origin login` to enable connected mode.

Project-level: `.origin.json` in repo root for per-project settings.

## Data Storage

All session data lives in your git repo:

```
.git/origin-session-<id>.json    # Active session state
.git/origin-local.db             # Local SQLite database
refs/notes/origin                # Attribution metadata (git notes)
origin-sessions branch           # Session history & transcripts
```

Nothing leaves your machine unless you `origin login` and connect to the platform.

## Documentation

Full documentation with examples: [getorigin.io/cli](https://getorigin.io/cli)

## License

MIT
