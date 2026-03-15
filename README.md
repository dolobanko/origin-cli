# Origin CLI

Command-line tool for [Origin](https://origin-platform.fly.dev) — AI agent governance platform.

Track, enforce policies, and audit AI coding sessions across Claude Code, Gemini CLI, Cursor, Copilot, and more.

## Install

```bash
npm i -g @origin-dev/cli
```

## Quick Start

```bash
origin login          # authenticate with your Origin server
origin init           # register machine, detect tools, install hooks
```

That's it — 2 commands. Everything is automatic from here. AI coding tools are auto-detected and hooks are installed.

## Commands

| Command | Description |
|---------|-------------|
| `origin login` | Authenticate with your Origin server |
| `origin init` | Register machine, detect tools, install hooks |
| `origin sessions` | List coding sessions |
| `origin session <id>` | View session details |
| `origin review <id>` | Review a session (approve/reject/flag) |
| `origin agents` | List registered agents |
| `origin repos` | List repositories |
| `origin policies` | List policies |
| `origin stats` | View dashboard stats |
| `origin audit` | View audit log |
| `origin hooks <event>` | Internal — called by AI tool hooks |

## How It Works

Origin's CLI installs hooks into your AI coding tools (Claude Code, Gemini CLI, etc.). These hooks fire on session lifecycle events:

- `session-start` — registers the session with Origin
- `user-prompt-submit` — captures prompts
- `pre-tool-use` — enforces policies (file restrictions, cost limits)
- `post-tool-use` — tracks branch changes, file modifications
- `stop` / `session-end` — finalizes the session

Policies are enforced in real-time. If an agent tries to access a restricted file, the hook blocks the tool call before it executes.

## License

MIT
