# OpenClaw Features → Claude Code Implementation Plan (Verified)

## Already Have (0 work needed)

| OpenClaw | Claude Code Equivalent | Status |
|----------|----------------------|--------|
| Markdown memory | `MEMORY.md` + `LEARNED.md` | ✅ Active |
| Skills system | `.claude/commands/*.md` | ✅ Active |
| Hooks system | `settings.json` hooks | ✅ Active |
| Browser automation | Playwright MCP | ✅ Active |
| Shell + file access | Bash + Edit/Write/Read | ✅ Native |
| Multi-agent routing | `Agent` tool (subagent types) | ✅ Active |
| Cron scheduler | `CronCreate` tool | ✅ Available |
| Canvas/A2UI | Stitch MCP + Excalidraw MCP | ✅ Active |
| GitHub integration | GitHub MCP | ✅ Active |
| Link understanding | WebFetch tool | ✅ Native |
| Media understanding | Claude vision (Read images) | ✅ Native |
| Context compaction | Claude Code auto-compression | ✅ Native |
| AGENTS.md pattern | CLAUDE.md (same concept) | ✅ Active |

## High-Value Gaps

### 1. ChromaDB / Vector Memory (P0)
**OpenClaw**: `chromadb-memory` skill — auto-recalls semantically relevant context before every agent turn using ChromaDB + Ollama `nomic-embed-text`.

**Build**: Use **Pinecone MCP** (already active!) to index `MEMORY.md` entries. Before each response, search Pinecone for relevant memories. This is literally MemoryX for Claude Code.

### 2. ACP Bridge — IDE Integration (P0 if using Zed)
**OpenClaw**: `openclaw acp` — exposes Gateway as ACP stdio server. Zed connects to it as a custom agent.

**Build**: Claude Code already IS an ACP-compatible agent. To integrate with Zed:
```json
{
  "agent_servers": {
    "Claude Code ACP": {
      "type": "custom",
      "command": "claude",
      "args": ["--acp"]
    }
  }
}
```
Monitor ACP SDK (`@agentclientprotocol/sdk`) for compatibility.

### 3. ClawHub-style Skill Index (P0)
**OpenClaw**: 13,729 community skills, tagged, searchable.

**Build**: `~/.claude/commands/INDEX.md` with tags, descriptions, trigger conditions. SessionStart hook loads it. Claude knows what skills exist before the user even asks.

### 4. QMD Memory Backend (P1)
**OpenClaw**: BM25 + vector + reranking, fully local, Bun + node-llama-cpp, auto-downloads GGUF.

**Build**: Use Pinecone MCP for vector. BM25 via simple grep. Reranking via Claude itself.

### 5. Agent Bitcoin Wallet (P2 — experimental)
**OpenClaw**: claw-cash — BTC treasury + x402 payments + Nitro Enclave keys.

**Build**: Integrate claw-cash `skills/` into Claude Code setup. Agent can pay for APIs, receive payment for work. Needs claw-cash daemon running.

### 6. Auto-Reply / Passive Agent (P1)
**OpenClaw**: Agent watches channels, replies without explicit invocation.

**Build**: Slack MCP + CronCreate — check Slack every N minutes, auto-process new messages.

### 7. Polls / Human-in-the-loop (P2)
**OpenClaw**: Native poll creation in Discord/Telegram/Slack.

**Build**: Slack MCP `create poll` + wait for response. Or simple y/n prompts via AskUserQuestion.

### 8. Sandboxed Execution (P1)
**OpenClaw**: `Dockerfile.sandbox` + `Dockerfile.sandbox-browser` — isolated shell + browser.

**Build**: Use Claude Code's built-in sandbox mode OR wrap Bash tool calls in a Docker container.

## The Core Insight (Updated)

OpenClaw's genius:
1. **Flat Markdown memory** — human-readable, git-trackable, zero infra
2. **ChromaDB layer on top** — semantic recall, but optional
3. **ACP bridge** — IDE → Gateway → LLM, clean separation
4. **Skills as npm packages** — versioned, composable, marketplace
5. **AGENTS.md as ground truth** — the agent reads it every session

We already have 1, 4 (via Claude skills), and 5 (via CLAUDE.md). 
Priority: add **2 (Pinecone semantic recall)** + **3 (skill index)** next.

## Action Plan

```
P0 (this week):
  [ ] Skill index: ~/.claude/commands/INDEX.md with tags
  [ ] Pinecone semantic memory: index MEMORY.md entries, query before each response
  [ ] SessionStart hook: load INDEX.md into context

P1 (next week):
  [ ] Auto-reply: CronCreate + Slack MCP poller
  [ ] QMD-lite: Pinecone + BM25 grep hybrid
  [ ] Sandbox mode: Docker wrapper for risky Bash calls

P2 (experimental):
  [ ] claw-cash integration: BTC wallet for agent autonomy
  [ ] ACP bridge: test Zed integration with Claude Code
  [ ] Polls: Slack MCP poll workflow
```
