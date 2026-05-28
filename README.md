# Bird Photography Recorder

> 🐦 Your personal birding archivist — identify, explain, and archive every bird you photograph.

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

## File Structure

```
workspace/
├── skills/bird-photography-recorder/    ← This skill
├── IDENTITY.md                          ← Agent identity template
├── SOUL.md                              ← Personality & workflow
├── AGENTS.md                            ← Workspace rules
├── TOOLS.md                             ← Bird resources & APIs
├── USER.md                              ← User context template
├── HEARTBEAT.md                         ← Periodic checks
└── bird-records/                        ← Generated archives
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

# Create workspace files from templates
cp references/IDENTITY.md.template ~/.openclaw/workspace-your-agent/IDENTITY.md
cp references/SOUL.md.template ~/.openclaw/workspace-your-agent/SOUL.md
# ... etc

# Restart OpenClaw Gateway
openclaw gateway restart
```

## Required APIs

- **iNaturalist API** — Free, no key needed for basic queries
  - `https://api.inaturalist.org/v1/taxa?q=<name>&rank=species`

## License

MIT — Go forth and archive every bird you meet 🐦📸
