# OpenClaw — Key Features Analysis

## 1. Skills System (Most Important)

- Skills = **Markdown packages** that extend agent capabilities
- 100+ available in **ClawHub** (official skill directory)
- Anyone can write + publish a skill
- Skills can: execute shell commands, manage filesystem, do web automation, call APIs
- Two extension models:
  - **Bundled**: shipped inside the `openclaw` npm package under `extensions/`
  - **Managed**: separate npm package, installed on demand

**Claude Code equivalent**: Claude Code skills system (`.claude/skills/`) — this is the SAME pattern.

## 2. Multi-Channel Inbox

- Single agent, 20+ inbound channels
- Route different accounts/channels to different isolated agents (workspaces)
- Per-agent sessions

**Claude Code equivalent**: hooks system + MCP servers acting as channel adapters.

## 3. Markdown Memory

- No vector DB — just `.md` files on disk
- Per-conversation + persistent global memory
- Human-editable, git-trackable
- Auto-saved on significant discoveries

**Claude Code equivalent**: `~/.claude/LEARNED.md` + `MEMORY.md` (already in use!).

## 4. Browser Automation

- First-class browser tool: navigate, fill forms, extract data
- Sandboxed or full-access mode

**Claude Code equivalent**: Playwright MCP server.

## 5. Shell + File Access

- Read/write files, execute shell commands, run scripts
- Sandboxed mode option

**Claude Code equivalent**: Bash tool + Edit/Write/Read tools.

## 6. Cron / Scheduled Tasks

- Built-in cron tool for recurring agent tasks
- Schedule any skill to run on interval

**Claude Code equivalent**: `CronCreate` tool (available in this session!).

## 7. Voice Interface

- Wake word + continuous voice
- ElevenLabs TTS + system fallback

**Claude Code equivalent**: Not natively available — but could integrate via macOS `say` + `osascript`.

## 8. Canvas / A2UI

- Agent-driven live visual workspace
- Nodes-based UI that agents can manipulate

**Claude Code equivalent**: Stitch MCP (UI generation), Excalidraw MCP (diagrams).

## 9. Multi-Agent Routing

- Route channels → isolated agents
- Each agent has own session + workspace

**Claude Code equivalent**: Claude Code subagents (Agent tool) with specialized roles.

## 10. ClawHub (Skill Marketplace)

- Public directory of 100+ community skills
- Install with one command
- Versioned, npm-based

**Claude Code equivalent**: Skills system already exists — ClawHub pattern = our skill files in `.claude/commands/`.
