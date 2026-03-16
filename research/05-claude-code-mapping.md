# OpenClaw Features → Claude Code Implementation Plan

## Already Have (0 work needed)

| OpenClaw Feature | Claude Code Equivalent | Status |
|-----------------|----------------------|--------|
| Markdown memory | `MEMORY.md` + `LEARNED.md` | ✅ Active |
| Skills system | `.claude/commands/*.md` | ✅ Active |
| Browser automation | Playwright MCP | ✅ Active |
| Shell + file access | Bash + Edit/Write/Read tools | ✅ Native |
| Multi-agent routing | `Agent` tool with subagent types | ✅ Active |
| Cron / scheduled | `CronCreate` tool | ✅ Available |
| Canvas / diagrams | Stitch MCP + Excalidraw MCP | ✅ Active |
| GitHub integration | GitHub MCP | ✅ Active |

## Gap Analysis — Can Build

| OpenClaw Feature | Gap | Claude Code Implementation |
|-----------------|-----|---------------------------|
| **ClawHub skill marketplace** | No public index of skills | Create a `skills/` catalog in this repo — curated, tagged, ready to copy |
| **Wake word / voice** | No voice interface | `osascript` + macOS `say` + `sox` for recording — partial implementation possible |
| **Multi-channel inbox** | Single chat interface | Hook system + MCP adapters for Slack/Discord/Telegram |
| **MemoryX (semantic search)** | Flat Markdown only | Add Pinecone MCP to index MEMORY.md for vector search |
| **Skill auto-discovery** | Manual skill loading | SessionStart hook that reads `skills/index.md` and loads context |
| **Agent social (Moltbook)** | N/A | Low priority for personal use |

## Priority Build List

### P0 — Immediate wins
1. **Skill catalog** — structured `skills/index.md` with tags (like ClawHub) so Claude knows what's available
2. **Semantic memory** — plug Pinecone into MEMORY.md workflow for "find related memories" queries
3. **Cron briefs** — use `CronCreate` to schedule daily brief (chess puzzle + code challenge + bio news)

### P1 — Medium effort
4. **Slack/Discord channel** — MCP adapter so Claude can receive messages from Slack as tasks
5. **Voice trigger** — `osascript` wake word → spawn Claude Code session (macOS only)

### P2 — Research needed
6. **DevClaw patterns** — study DevClaw's GitHub integration for PR review automation
7. **MemoryX vector layer** — Pinecone-backed semantic search over conversation history

## The Core Insight

OpenClaw's secret is not any single feature — it's the **composability**:
> One gateway + Markdown skills + flat memory + any LLM = infinitely extensible personal AI

Claude Code already has all the primitives. The missing piece is **discoverability** —
a ClawHub-style index so the agent always knows what tools/skills it has.

That's what we build next.
