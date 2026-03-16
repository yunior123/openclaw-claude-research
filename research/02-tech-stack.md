# OpenClaw — Tech Stack (Verified from actual repo)

> Source: `github.com/openclaw/openclaw` read directly via GitHub API

## Runtime

| Layer | Technology | Notes |
|-------|------------|-------|
| Language | **TypeScript** (ESM strict) | No `any`, no `@ts-nocheck` |
| Runtime | **Node.js 22+** / **Bun** | Bun preferred for dev/scripts/tests |
| Package manager | **pnpm** (primary) | `pnpm-lock.yaml`; Bun also supported |
| Build | **tsdown** | Fast TS bundler |
| Module resolution | **jiti** | Runtime TS resolution for plugins |
| Lint | **oxlint** | Rust-based, fast |
| Format | **oxfmt** | Rust-based |
| Dead code | **knip** | `knip.config.ts` |
| Copy-paste detect | **jscpd** | `.jscpd.json` |
| Secret scanning | **detect-secrets** | `.secrets.baseline` (433KB!) |
| GH Actions security | **zizmor** | `zizmor.yml` |
| Pre-commit | **prek** | Runs same checks as CI |
| Test framework | **Vitest** | V8 coverage, 70% threshold |
| Python tooling | **pyproject.toml** | Scripts/tooling layer |

## Monorepo Structure

```
openclaw/
├── src/                  # Core TypeScript source
├── apps/                 # iOS, Android, macOS native apps
├── extensions/           # Channel plugins (workspace packages)
├── packages/
│   ├── clawdbot/         # Legacy compatibility shim
│   └── moltbot/          # Legacy compatibility shim
├── skills/               # Bundled skills (in-repo)
├── ui/                   # Web UI
├── vendor/               # Vendored deps
├── .agent/               # Agent config
└── .agents/              # Multi-agent config
```

## src/ Module Map (key directories)

| Module | Purpose |
|--------|---------|
| `src/acp/` | **ACP bridge** — stdio protocol for IDE integration (Zed, etc.) |
| `src/agents/` | Agent management + multi-agent routing |
| `src/auto-reply/` | Auto-reply logic |
| `src/browser/` | Browser automation |
| `src/canvas-host/` | A2UI canvas host |
| `src/channels/` | Channel abstraction layer |
| `src/cli/` | CLI wiring (`osc-progress` + `@clack/prompts`) |
| `src/commands/` | CLI commands |
| `src/context-engine/` | **Context management system** |
| `src/cron/` | Built-in cron scheduler |
| `src/daemon/` | Daemon mode |
| `src/gateway/` | Gateway core (WebSocket, session routing) |
| `src/hooks/` | Hooks system |
| `src/i18n/` | Internationalization (zh-CN generated) |
| `src/interactive/` | Interactive REPL mode |
| `src/link-understanding/` | Link/URL processing |
| `src/markdown/` | Markdown processing |
| `src/media/` + `src/media-understanding/` | Media pipeline |
| `src/memory/` | Memory system (core: flat Markdown) |
| `src/node-host/` | Node execution host |
| `src/pairing/` | Device pairing |
| `src/plugin-sdk/` + `src/plugin-sdk-internal/` | Plugin SDK |
| `src/plugins/` | Plugin loader/manager |
| `src/providers/` | LLM provider adapters |
| `src/routing/` | Message routing engine |
| `src/secrets/` | Secrets management |
| `src/security/` | Security layer |
| `src/sessions/` | Session store + compaction |
| `src/terminal/` | Terminal UI utils (`palette.ts`, `table.ts`) |
| `src/tts/` | TTS (ElevenLabs + system fallback) |
| `src/tui/` | TUI components |
| `src/whatsapp/` | WhatsApp channel |
| `src/wizard/` | Setup wizard |

## Native Apps

- **macOS**: Swift/SwiftUI, `@Observable` framework, Sparkle auto-update (`appcast.xml`)
- **iOS**: SwiftUI, TestFlight not provided (build from source)
- **Android**: `apps/android/app/build.gradle.kts`
- Linting: `.swiftlint.yml` + `.swiftformat`

## Deployment Options

| Method | Config |
|--------|--------|
| Docker Compose | `docker-compose.yml` (standard) |
| Dockerfile.sandbox | Sandboxed shell execution |
| Dockerfile.sandbox-browser | Sandboxed browser |
| Fly.io | `fly.toml` + `fly.private.toml` |
| Render | `render.yaml` |
| Cloudflare Workers | via **Moltworker** (separate repo) |
| Podman | `setup-podman.sh` + `openclaw.podman.env` |

## LLM Providers

- Anthropic (Claude) — native
- OpenAI (GPT-4/5) — native
- Any OpenAI-compatible API — custom provider (`src/providers/`)
- Ollama — via custom provider config (used by ChromaDB memory skill)

## Memory Architecture (CORRECTED)

**Core**: Flat Markdown files at `~/.openclaw/agents/<id>/sessions/*.jsonl`

**Optional plugins** (NOT built-in):
1. **chromadb-memory** skill — ChromaDB + Ollama `nomic-embed-text` embeddings, auto-recall before every agent turn
2. **QMD backend** — `memory.backend = "qmd"` — BM25 + vectors + reranking, Bun + node-llama-cpp, auto-downloads GGUF from HuggingFace
3. **Hybrid Memory** (`n00n0i/openclaw-hybrid-memory`) — ChromaDB + Memgraph (vector + graph)
4. **MemoryX** (`@t0ken.ai/memoryx-openclaw-plugin`) — advanced memory plugin

> ChromaDB is NOT a core dependency. It's a community skill installed separately.

## ACP (Agent Client Protocol) — CRITICAL FINDING

`openclaw acp` exposes an ACP agent over **stdio**, routing IDE prompts to the Gateway via WebSocket.

- SDK: `@agentclientprotocol/sdk` 0.15.x
- Works with **Zed** editor natively
- Session mapping: `acp:<uuid>` → Gateway session key
- Supports: `initialize`, `newSession`, `loadSession`, `prompt`, `cancel`, `listSessions`
- Config in Zed: add `OpenClaw ACP` as custom agent server running `openclaw acp`

**This is directly mappable to Claude Code's MCP stdio protocol.**
