# OpenClaw AGENTS.md — Lessons for Our CLAUDE.md

> OpenClaw's `AGENTS.md` is 35KB. It's the ground truth for every AI agent working in the repo.
> Source: `github.com/openclaw/openclaw/AGENTS.md`

## Key Patterns Worth Adopting

### 1. Explicit module map
OpenClaw documents EXACTLY where things live:
```
Source code: src/ (CLI in src/cli, commands in src/commands, ...)
Plugins: extensions/*
Docs: docs/
```
→ **Apply to CLAUDE.md**: add a `## Module Map` section pointing to key dirs in each project.

### 2. Platform-specific test commands
Separate commands for unit, e2e, live, docker, parallel smoke:
```bash
pnpm test                         # unit
pnpm test:live                    # real keys
pnpm test:docker:live-gateway     # Docker E2E
pnpm test:parallels:macos         # macOS smoke
pnpm test:parallels:windows       # Windows smoke
pnpm test:parallels:linux         # Linux smoke
```
→ **Apply**: document exact test commands per project in CLAUDE.md.

### 3. Multi-agent safety rules (CRITICAL)
OpenClaw has explicit rules for when multiple agents touch the same repo:
- Never `git stash` unless asked
- Never switch branches unless asked
- Never create/remove git worktrees unless asked
- When "push": `git pull --rebase` first (never discard other agents' work)
- When "commit": scope to YOUR changes only
- When "commit all": group in chunks
→ **Already in our CLAUDE.md** — but we should be more explicit.

### 4. Auto-close labels for issues
OpenClaw uses `r:skill`, `r:support`, `r:spam` labels to trigger automated close+comment.
→ **Apply**: create label taxonomy in origna-gta for common issue types.

### 5. PR truthfulness gates
Never merge a bug-fix PR based only on AI rationale. Minimum gate:
1. Symptom evidence (repro/log/failing test)
2. Verified root cause in code with file:line
3. Fix touches the implicated code path
4. Regression test (fail before / pass after)
→ **Apply to our PR review workflow.**

### 6. Version locations exhaustively documented
OpenClaw lists EVERY file where version must be bumped:
- `package.json`
- `apps/android/app/build.gradle.kts`
- `apps/ios/Sources/Info.plist` + `apps/ios/Tests/Info.plist`
- `apps/macos/Sources/OpenClaw/Resources/Info.plist`
→ **Apply**: document same for origna_gta (pubspec.yaml, etc.)

### 7. Tool schema guardrails
For Google Antigravity (Gemini) tool schemas:
- Avoid `Type.Union`, `anyOf`, `oneOf`, `allOf`
- Use `stringEnum`/`optionalStringEnum` for string lists
- Use `Type.Optional(...)` instead of `... | null`
→ **Apply when building MCP tools.**

### 8. Never edit `node_modules`
Explicit in AGENTS.md. Same applies to us.

### 9. Changelog hygiene
- User-facing changes only
- Append to END of section (not top)
- At most one contributor mention per line
→ **Apply to orignabase CHANGELOG.**

### 10. `openclaw acp --session agent:main:main` pattern
Target specific agents by session key from IDE.
→ **Insight**: Claude Code could support session targeting too — use `--resume <conversation_id>`.

## claw-cash CLAUDE.md — Minimalist Agent Instructions

> Only 3 rules, each essential:

1. **Core principle**: Agents hold BTC, swap to stablecoins on demand. Never bypass this flow.
2. **Landing page sync**: Always keep `index.html` and `index.md` in sync. The `.md` is machine-readable (served via `<link rel="alternate" type="text/markdown">`).
3. No fluff.

**Lesson**: CLAUDE.md should be densely meaningful, not exhaustive. Every line must change behavior.

## Machine-Readable Landing Pages

claw-cash uses `<link rel="alternate" type="text/markdown">` in `index.html` to serve a Markdown version for agents. This is a pattern we should adopt:
- Any page Claude agents might visit → add `?format=md` or `<link rel="alternate">` Markdown version
- Reduces token cost of parsing HTML
