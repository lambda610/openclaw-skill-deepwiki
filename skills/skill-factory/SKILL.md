---
name: skill-factory
description: Create skills compatible with both OpenClaw and Claude Code (Agent Skills). Use when building a new skill that supports dual platforms with unified YAML frontmatter, proper validation, and OpenClaw workflows.
---

# Skill Factory

Create dual-compatible skills following both **Agent Skills spec** (agentskills.io) and **OpenClaw** specifications.

## Core Understanding

### Two Ecosystems, One Skill

| Aspect | Agent Skills (Claude Code) | OpenClaw |
|--------|---------------------------|----------|
| **Trigger** | YAML frontmatter `description` matches user message | Commands via `/skill-cmd` |
| **Validation** | `skills-ref validate` | `quick_validate.py` |
| **Optional dirs** | `scripts/`, `references/`, `assets/` | `workflows/`, `scripts/`, `references/`, `prompts/` |
| **Key extension** | - | `metadata.openclaw` for gating |

### Unified Approach

```yaml
# Always include this for both platforms
---
name: skill-name              # kebab-case, max 64 chars
description: Clear trigger... # max 1024 chars
license: Apache-2.0           # optional
compatibility:                 # optional, max 500 chars
metadata:                     # optional, client-specific
  openclaw:                   # OpenClaw-specific gating
    requires:
      bins: []
      env: []
      config: []
---
```

## Workflow: Creating a New Skill

### Step 1: Initialize

Use the OpenClaw init script or create manually:

```bash
# Using OpenClaw's init_skill.py
python3 /path/to/init_skill.py my-skill --path ./skills --resources scripts,references --examples

# Manual creation
mkdir -p my-skill/{scripts,references,assets}
```

### Step 2: Write SKILL.md

Follow the spec exactly:

```yaml
---
name: my-skill
description: Comprehensive description of what this skill does. Include specific triggers: (1) When user wants X, (2) For Y tasks, (3) Working with Z files.
metadata:
  {
    "openclaw": {
      "requires": { "bins": ["python3"], "env": ["API_KEY"] },
      "emoji": "🛠"
    }
  }
---

# My Skill

## Overview

[What this enables]

## When to Use

- User asks for [specific task]
- Working with [file type/domain]

## Bundled Resources

- **scripts/**: Executable utilities
- **references/**: Detailed documentation
- **assets/**: Templates
```

### Step 3: Validate

```bash
# Agent Skills validation
skills-ref validate ./my-skill

# OpenClaw validation
python3 quick_validate.py ./my-skill
```

### Step 4: Package (OpenClaw)

```bash
python3 package_skill.py ./my-skill ./dist
```

## Validation Rules (from Specs)

### Name Validation

- **Max length**: 64 characters
- **Format**: lowercase, kebab-case (letters, digits, hyphens)
- **Rules**:
  - Must be lowercase
  - Cannot start/end with hyphen
  - No consecutive hyphens
  - Directory name must match skill name
  - Supports i18n (Unicode letters via NFKC normalization)

### Description Validation

- **Max length**: 1024 characters
- **Rules**:
  - Must be non-empty string
  - No angle brackets (`<` or `>`) - triggers XSS in some contexts

### Metadata Fields

**Allowed (Agent Skills)**:
- `name`, `description`, `license`, `compatibility`, `allowed-tools`, `metadata`

**Allowed (OpenClaw extension)**:
```yaml
metadata:
  {
    "openclaw": {
      "always": true,
      "emoji": "🎨",
      "homepage": "https://...",
      "os": ["darwin", "linux"],
      "requires": {
        "bins": [],
        "anyBins": [],
        "env": [],
        "config": []
      },
      "primaryEnv": "API_KEY",
      "install": [...]
    }
  }
```

## Progressive Disclosure Pattern

For detailed guidance, see:
- [SPEC.md](references/SPEC.md) - Complete dual-platform spec
- [VALIDATION.md](references/VALIDATION.md) - Validation rules
- [TEMPLATES.md](references/TEMPLATES.md) - Ready-to-use templates
- [EXAMPLES.md](references/EXAMPLES.md) - Real-world skills
- [WORKFLOWS.md](references/WORKFLOWS.md) - OpenClaw workflow patterns

## Common Patterns

### Pattern 1: Agent Skills Only

```
my-skill/
├── SKILL.yaml              # YAML frontmatter + body
├── scripts/                # Executable code
└── references/             # Documentation
```

### Pattern 2: OpenClaw with Commands

```
my-skill/
├── SKILL.md
├── workflows/              # Command definitions
│   ├── command1.yaml
│   └── command2.yaml
└── scripts/
```

### Pattern 3: Dual-Compatible (Recommended)

```
my-skill/
├── SKILL.md               # Works on both platforms
├── workflows/             # OpenClaw commands (ignored by Claude Code)
├── scripts/               # Shared executables
├── references/            # Documentation
└── assets/                # Templates
```

## Key Principles

1. **YAML Frontmatter Required** - Always include for Agent Skills compatibility
2. **OpenClaw metadata optional** - Add only if you need gating/install
3. **Avoid prompts/** - Not part of Agent Skills spec
4. **Keep SKILL.md lean** - Use references/ for detailed docs
5. **One repo, both platforms** - Agent Skills ignores `workflows/` and `prompts/`
