# OpenClaw — Forks, Derivatives & Ecosystem (Verified)

## Official OpenClaw Org Repos

| Repo | Purpose |
|------|---------|
| `openclaw/openclaw` | Core agent (this repo) |
| `openclaw/skills` | Official skill registry (ClawHub) |
| `openclaw/clawhub` | Skill marketplace front-end |
| `openclaw/nix-steipete-tools` | Nix packaging |
| `openclaw/maintainers` | Private release/signing docs |

## Legacy Packages (in-repo)

- `packages/clawdbot/` — backward-compat shim for `clawdbot` name
- `packages/moltbot/` — backward-compat shim for `moltbot` name

## Top Forks by Stars (March 2026)

| Project | Stars | Language | Key Difference |
|---------|-------|----------|----------------|
| **OpenFang** | +3,915 | — | Fang-themed variant |
| **IronClaw** | +3,902 | Rust | Security-hardened, air-gapped |
| **NanoClaw** | +3,031 | TypeScript | Minimal, single-file deploy |
| **Nanobot** | +3,022 | — | Ultra-lightweight |
| **ZeroClaw** | +2,668 | Go | No Node.js dependency |
| **PicoClaw** | +1,411 | Rust | Sub-10ms latency |
| **ClawWork** | +773 | — | AI coworker, $15K in 11h demo |

## Notable Ecosystem Projects

### Moltworker (Cloudflare)
`github.com/cloudflare/moltworker`
- Run OpenClaw on **Cloudflare Workers**
- Stateless, serverless gateway
- Shows the gateway pattern works without persistent processes

### claw-cash (tiero)
`github.com/tiero/claw-cash`
- **Bitcoin wallet for AI agents**
- Architecture: BTC treasury → swap to stablecoins on demand
- Key management: **AWS Nitro Enclaves** (hardware security)
- Payment protocols: x402, Lightning invoices, stablecoin sends
- Swap infra: LendaSwap + Boltz
- Has `skills/` bundling OpenClaw skills
- Uses **CLAUDE.md** — built with Claude Code
- Landing page has `<link rel="alternate" type="text/markdown">` — machine-readable for agents

### CashClaw (ertugrulakben)
`github.com/ertugrulakben/cashclaw`
- OpenClaw middleware for autonomous freelance agents
- 12 skill packs: SEO, content, leads, email, competitor analysis, landing pages, scraping, invoicing
- Integrates: Stripe, HYRVEai marketplace
- Use case: agent wakes → checks pipeline → does work → invoices → collects payment

### ClawWork (HKUDS)
`github.com/HKUDS/ClawWork`
- OpenClaw as AI coworker
- 220 GDP validation tasks across 44 economic sectors
- $10 starting budget + pay-per-token pressure
- Claim: $15,000 earned in 11 hours

### ClawRag (2dogsandanerd)
`github.com/2dogsandanerd/ClawRag`
- RAG system: **Docling** document processing + **ChromaDB** vector storage
- Turns OpenClaw into a knowledge base agent

### claw0 (shareAI-lab)
`github.com/shareAI-lab/claw0`
- Learn OpenClaw from scratch — sections to build an agent from zero
- Educational resource

### openclaw-hybrid-memory (n00n0i)
`github.com/n00n0i/openclaw-hybrid-memory`
- ChromaDB + Memgraph (vector + graph)
- Find semantically similar text AND entity relationships

### awesome-openclaw-skills (VoltAgent)
`github.com/VoltAgent/awesome-openclaw-skills`
- 5,400+ skills filtered + categorized from ClawHub
- Searchable index

### rohitg00/awesome-openclaw
`github.com/rohitg00/awesome-openclaw`
- Curated list of OpenClaw resources, tutorials, integrations

## Key Architectural Lessons from Derivatives

1. **Moltworker** proves: gateway = stateless message router, can run serverless
2. **IronClaw/PicoClaw** (Rust) validate architecture quality — same patterns, 10x perf
3. **claw-cash** shows: agents can hold money + transact autonomously with hardware-secure keys
4. **CashClaw** shows: skills + Stripe + marketplace = autonomous income generation
5. **ClawRag** shows: add Docling + ChromaDB = private knowledge RAG with zero cloud
6. **ClawWork** shows: economic pressure (pay-per-token) + GDP tasks = measurable agent ROI
