# OpenClaw — Overview

## Origin & Rebrand History

| Date | Name | Notes |
|------|------|-------|
| 2025 | **Clawdbot** | Initial project by Peter Steinberger |
| Late 2025 | **Moltbot** | First rebrand |
| Jan 30, 2026 | **OpenClaw** | Final rebrand — viral explosion begins |

Went from 0 → 300,000+ stars in ~45 days. Beat React's 10-year record in 60 days. Surpassed Linux.

## What It Is

A **self-hosted personal AI agent** that:
- Runs as a long-running Node.js service on your machine
- Routes messages from 20+ chat platforms → your chosen LLM
- Executes real-world tasks via Skills (browser, shell, files, APIs)
- Keeps memory as plain Markdown files — no vector DB, no cloud sync

## Design Philosophy

- **Local-first**: everything runs on your hardware
- **Privacy-first**: no data leaves your machine unless you configure it to
- **Intentionally simple**: no vector DB, no multi-agent orchestration frameworks
- **Composable**: Skills = Markdown packages anyone can write and share
- **One gateway**: single control plane for sessions, channels, tools, events

## Creator

Peter Steinberger — known in iOS/macOS dev community (PSPDFKit founder).
