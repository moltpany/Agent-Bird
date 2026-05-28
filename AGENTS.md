# Agent-Bird · Repository Instructions

## Purpose

This repository is a **skill library** for bird photography archiving agents. It is not a runnable agent itself — it provides skills and workspace templates that users copy into their own OpenClaw agent workspace.

## Repository Layout

```
agent-bird/
├── skills/
│   ├── bird-photography-recorder/   ← Core identification + archiving skill
│   └── bird-diary-publisher/        ← Publish sightings to Bird Diary
├── references/                      ← Workspace templates (copy, don't edit in place)
│   ├── IDENTITY.md.template
│   ├── SOUL.md.template
│   ├── AGENTS.md.template
│   ├── TOOLS.md.template
│   ├── USER.md.template
│   └── HEARTBEAT.md.template
└── README.md
```

## How Skills Relate

```
bird-photography-recorder  →  archives locally
         ↓ (user confirms)
bird-diary-publisher       →  publishes to https://moltpany.github.io/bird-diary/
```

## Contributing a New Skill

- One skill per directory under `skills/`.
- Every skill directory must contain a `SKILL.md` with a YAML frontmatter block (`name`, `description`).
- Skills must be stateless — all state lives in the user's workspace (`bird-records/`, `stats.json`).
- Do not hardcode usernames, camera models, or personal data in skills.

## Updating Templates

- Templates in `references/` are starting points. Users customize them after copying.
- Use `{{ user.field }}` notation for values that come from USER.md.
- Keep templates minimal — only include what the agent genuinely needs at runtime.

## External Work

- Bird Diary public site: `https://moltpany.github.io/bird-diary/`
- Bird Diary repository: `https://github.com/moltpany/bird-diary`
- Moltpany platform: `https://moltpany.github.io/`
