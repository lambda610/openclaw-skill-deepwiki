# Skill Factory

> Create dual-platform skills through natural conversation

A skill for building skills that work on both **OpenClaw** and **Claude Code** (Agent Skills). Just tell it what you want to build.

## Features

- **Zero Config** — No Python, no setup, just describe what you need
- **Conversational** — Describe your skill in natural language
- **Dual-Platform** — Output works on OpenClaw and Claude Code
- **Guided Flow** — Interactive step-by-step creation
- **Spec Compliant** — Follows Agent Skills specification automatically

## Quick Start

Tell it what you want:

```
I want to create a skill
Help me build a dual-platform skill
Make a skill for weather lookup
```

The skill will guide you through:
1. **Name & Purpose** — What should the skill do?
2. **Platforms** — OpenClaw only, Claude Code, or both?
3. **Features** — Commands, scripts, references needed?
4. **Output** — It creates the directory structure and SKILL.md for you

## What It Creates

```
my-skill/
├── SKILL.md              # Ready to use
├── workflows/            # OpenClaw commands (if needed)
├── scripts/              # Executables (if needed)
└── references/           # Docs (if needed)
```

## Platform Support

| Platform | Trigger | Commands | Scripts |
|----------|---------|----------|---------|
| OpenClaw | `/skill-name` | ✅ | ✅ |
| Claude Code | Natural language | ❌ | ✅ |
| Both | SKILL.md frontmatter | ✅ | ✅ |

## Why Dual-Platform?

- One repo, two platforms
- Agent Skills ignores `workflows/` (OpenClaw specific)
- OpenClaw reads `SKILL.md` and `workflows/`
- Shared resources in `scripts/` and `references/`

## License

Apache License 2.0 — See [LICENSE](LICENSE)
