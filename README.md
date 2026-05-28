# Agent-Bird

> 🐦 Your personal birding archivist — identify, explain, and archive every bird you photograph.

**version:** v0.1 · 0 molts

## What Is This?

A framework for turning bird photos into a **rich, searchable archive** with species profiles, sighting logs, and future IP potential.

## Core Workflow

When you send a bird photo:

1. 📸 **Save** — Photo archived with timestamp
2. 🔍 **Identify** — Species name (中文/英文/科学名) via visual analysis + iNaturalist API
3. 📝 **Explain** — Fun facts, habitat, behavior, conservation status
4. 💾 **Log** — Daily journal + species profile updated automatically
5. 📊 **Track** — Running stats (total species, lifers, rarest sighting)
6. 🎴 **(Future)** — Card drafts, field guide material

## Trigger Phrases

- "这是什么鸟"
- "认鸟"
- "记录这只鸟"
- "存档"
- Send any bird photo

## Repository Layout

```
agent-bird/
├── skills/bird-photography-recorder/    ← The skill definition (SKILL.md)
└── references/                          ← Templates to copy into your workspace
    ├── IDENTITY.md.template
    ├── SOUL.md.template
    ├── AGENTS.md.template
    ├── TOOLS.md.template
    ├── USER.md.template
    └── HEARTBEAT.md.template
```

After setup, your agent workspace will look like:

```
your-workspace/
├── skills/bird-photography-recorder/    ← copied from this repo
├── IDENTITY.md                          ← from IDENTITY.md.template
├── SOUL.md                              ← from SOUL.md.template
├── AGENTS.md                            ← from AGENTS.md.template
├── TOOLS.md                             ← from TOOLS.md.template
├── USER.md                              ← from USER.md.template
├── HEARTBEAT.md                         ← from HEARTBEAT.md.template
└── bird-records/                        ← generated at runtime
    ├── raw-photos/
    ├── sightings-log/
    ├── by-species/
    │   └── <species>/
    │       ├── photos/
    │       └── profile.md
    ├── stats.json
    └── ip-assets/
        └── cards/
```

## Quick Start

```bash
# Copy skill to your agent workspace
cp -r skills/bird-photography-recorder ~/.openclaw/workspace-your-agent/skills/

# Copy all workspace templates
cp references/IDENTITY.md.template   ~/.openclaw/workspace-your-agent/IDENTITY.md
cp references/SOUL.md.template       ~/.openclaw/workspace-your-agent/SOUL.md
cp references/AGENTS.md.template     ~/.openclaw/workspace-your-agent/AGENTS.md
cp references/TOOLS.md.template      ~/.openclaw/workspace-your-agent/TOOLS.md
cp references/USER.md.template       ~/.openclaw/workspace-your-agent/USER.md
cp references/HEARTBEAT.md.template  ~/.openclaw/workspace-your-agent/HEARTBEAT.md

# Edit USER.md to fill in your name, device, location, etc.

# Restart OpenClaw Gateway
openclaw gateway restart
```

## Required APIs

- **iNaturalist API** — Free, no key needed for basic queries
  - `https://api.inaturalist.org/v1/taxa?q=<name>&rank=species`

## License

MIT — Go forth and archive every bird you meet 🐦📸
