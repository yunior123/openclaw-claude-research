# OpenClaw — Tech Stack Under the Hood

## Runtime

| Layer | Technology |
|-------|------------|
| Language | **TypeScript** |
| Runtime | **Node.js 22+** (Node 24+ for custom skill dev) |
| Package manager | npm / pnpm / bun |
| Deployment | **Docker Compose** (standard) or bare Node |
| Build tool | `tsdown` (fast TypeScript bundler) |
| Module resolution | `jiti` (runtime TS resolution for plugins) |

## Plugin / Skill SDK

- Distributed as **subpath exports** from the main `openclaw` npm package
- `openclaw/plugin-sdk/core` — startup-critical APIs
- `openclaw/plugin-sdk/<channel>` — per-channel lazy imports
- Build pipeline:
  1. `tsdown-build.mjs` — compiles TypeScript → JavaScript
  2. `copy-plugin-sdk-root-alias.mjs` — creates SDK module structure
  3. `tsconfig.plugin-sdk.dts.json` — generates `.d.ts` declaration files
  4. `write-plugin-sdk-entry-dts.ts` — main SDK entry point types
- Plugin deps go in `dependencies` (not `devDependencies`) — required by runtime

## LLM Providers Supported

- **Anthropic** (Claude) — native
- **OpenAI** (GPT-4/5) — native
- **Any OpenAI-compatible API** — custom provider config (5-min setup)
- Recommendation: use strongest latest-gen model for lowest prompt-injection risk

## Memory Architecture

- **Flat Markdown files on disk** — intentional, no vector DB
- Per-agent session files
- Human-readable, git-trackable, portable
- No cloud dependency

## Channels Architecture

- Gateway = single control plane
- Channels decouple from models
- 20+ supported: WhatsApp, Telegram, Slack, Discord, Signal, iMessage (BlueBubbles), IRC, MS Teams, Matrix, LINE, Mattermost, Twitch, Nostr, Zalo, WebChat, macOS, iOS/Android

## Voice

- Wake word detection on macOS/iOS
- Continuous voice on Android
- TTS: ElevenLabs (primary) + system TTS fallback

## Canvas / Visual

- **A2UI** — agent-driven visual workspace
- Live Canvas: agents can render nodes, draw, update UI in real-time

## Infrastructure

- Self-hosted via Docker Compose
- Cloudflare Workers port: **Moltworker** (by Cloudflare team)
- No mandatory cloud service
