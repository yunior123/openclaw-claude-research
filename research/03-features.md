# OpenClaw — Key Features (Verified from source)

## 1. Skills System

- Skills = **npm packages** with TypeScript exports (not just Markdown!)
- Two distribution models:
  - **Bundled**: in `extensions/` or `skills/` dir — workspace packages
  - **Managed**: separate npm package, installed on demand
- Plugin SDK: `openclaw/plugin-sdk/core` + per-channel subpath exports
- Runtime resolves via **jiti** alias: `openclaw/plugin-sdk`
- Plugin deps must be in `dependencies` (not `devDependencies`)
- **13,729 community skills** in ClawHub as of Feb 28, 2026
- `awesome-openclaw-skills` repo by VoltAgent: 5,400+ curated/categorized skills

**Claude Code equivalent**: `.claude/commands/*.md` skills + MCP servers

## 2. Hooks System (`src/hooks/`)

- OpenClaw has a full hooks system (same concept as Claude Code hooks)
- Pre/post event hooks on agent turns, tool calls, message delivery
- Used for: auto-reply, filtering, routing, logging

**Claude Code equivalent**: Claude Code hooks in `settings.json` — identical pattern.

## 3. Context Engine (`src/context-engine/`)

- Dedicated module for context window management
- Session compaction (see `docs/reference/session-management-compaction.md`)
- JSONL session logs per agent: `~/.openclaw/agents/<agentId>/sessions/*.jsonl`

**Claude Code equivalent**: Claude Code's own context compression.

## 4. Multi-Channel Routing (`src/routing/`)

Built-in channels (core `src/`):
- `src/telegram`, `src/discord`, `src/slack`, `src/signal`, `src/imessage`
- `src/web` (WhatsApp web), `src/whatsapp/`, `src/channels/`

Extension channels (`extensions/`):
- `extensions/msteams`, `extensions/matrix`, `extensions/zalo`
- `extensions/zalouser`, `extensions/voice-call`

**Claude Code equivalent**: Slack/Discord MCP servers + webhook receivers.

## 5. ACP Bridge (`src/acp/`) — KEY FEATURE

Exposes Gateway as an **Agent Client Protocol** server over stdio.
- IDEs (Zed) connect via `openclaw acp`
- Routes IDE prompts → Gateway WebSocket → LLM
- Session mapping, streaming events, tool_call updates
- Multi-agent session targeting: `openclaw acp --session agent:main:main`

**Claude Code equivalent**: Claude Code IS the ACP-compatible agent. Could expose Claude Code as ACP server for Zed.

## 6. Gateway (`src/gateway/`)

- Long-running WebSocket server
- Single control plane: sessions, channels, tools, events
- `gateway.remote.url` + token auth
- `openclaw gateway run --bind loopback --port 18789 --force`
- Status: `openclaw gateway status --deep --require-rpc`

## 7. Memory System (`src/memory/`)

Core: flat Markdown + JSONL session logs
Optional plugins:
- **chromadb-memory**: ChromaDB + Ollama embeddings (community skill)
- **QMD**: BM25 + vectors + GGUF (built-in backend option, `memory.backend = "qmd"`)
- **MemoryX**: `@t0ken.ai/memoryx-openclaw-plugin`
- **Hybrid**: ChromaDB + Memgraph graph DB

## 8. Browser Automation (`src/browser/`)

- Sandboxed: `Dockerfile.sandbox-browser`
- Full access or sandboxed mode
- First-class tool alongside shell, files, cron

**Claude Code equivalent**: Playwright MCP server.

## 9. Cron Scheduler (`src/cron/`)

- Built-in cron for recurring agent tasks
- Schedule any skill on interval

**Claude Code equivalent**: `CronCreate` tool.

## 10. TTS (`src/tts/`)

- ElevenLabs primary
- System TTS fallback
- Voice wake word on macOS/iOS
- Continuous voice on Android

**Claude Code equivalent**: macOS `say` command + `osascript`.

## 11. Canvas / A2UI (`src/canvas-host/`)

- Agent-driven live visual workspace
- Bundle: `src/canvas-host/a2ui/.bundle.hash`
- Regenerate: `pnpm canvas:a2ui:bundle`

**Claude Code equivalent**: Stitch MCP + Excalidraw MCP.

## 12. Auto-Reply (`src/auto-reply/`)

- Automatic reply triggers without explicit invocation
- Rules-based + AI-driven

**Claude Code equivalent**: Claude Code hooks (PostToolUse, PreToolUse).

## 13. Link Understanding (`src/link-understanding/`)

- Processes URLs in messages — fetches, summarizes, extracts

**Claude Code equivalent**: WebFetch tool.

## 14. Media Understanding (`src/media-understanding/`)

- Vision: images → descriptions
- Audio processing pipeline

**Claude Code equivalent**: Claude's native vision (Read tool on images).

## 15. Polls (`src/polls.ts`)

- Native poll creation in supported channels
- Useful for human-in-the-loop decisions

## 16. Security Layer

- `src/security/` + `src/secrets/`
- `detect-secrets` scanning on every commit
- `zizmor` for GitHub Actions security
- `SECURITY.md` with trust model and design boundaries
