# Complete Dual-Platform Specification

This document details the specifications for creating skills compatible with both OpenClaw and Claude Code (Agent Skills).

---

## Agent Skills Spec (agentskills.io)

### Directory Structure

```
skill-name/
└── SKILL.md              # Required: YAML frontmatter + Markdown body
```

### Optional Directories

| Directory | Purpose | Content Type |
|-----------|---------|--------------|
| `scripts/` | Executable code | Python, Bash, JS |
| `references/` | Documentation | Markdown files |
| `assets/` | Static files | Templates, images, fonts |

### YAML Frontmatter

```yaml
---
name: skill-name          # Required, kebab-case, max 64 chars
description: ...          # Required, max 1024 chars
license: Apache-2.0       # Optional
compatibility: ...        # Optional, max 500 chars
allowed-tools: ...         # Optional, experimental
metadata: {}              # Optional, client-specific
---
```

### Validation (skills-ref)

```bash
# Install
pip install skills-ref

# Validate
skills-ref validate ./my-skill

# Read properties
skills-ref read-properties ./my-skill

# Generate prompt XML
skills-ref to-prompt ./skill-a ./skill-b
```

---

## OpenClaw Specification

### Directory Structure

```
skill-name/
├── SKILL.md              # Required
├── workflows/            # Optional: Command definitions
├── scripts/              # Optional: Executable code
├── references/          # Optional: Documentation
├── prompts/             # Optional: AI prompts
└── assets/              # Optional: Static files
```

### SKILL.md Format

OpenClaw follows Agent Skills spec with extensions:

```yaml
---
name: skill-name
description: ...
metadata:
  {
    "openclaw": {
      "requires": { "bins": [], "env": [], "config": [] },
      "always": true,
      "emoji": "🎨",
      "os": ["darwin", "linux"],
      "homepage": "https://...",
      "primaryEnv": "API_KEY",
      "install": [...]
    }
  }
---
```

### Workflows (OpenClaw-specific)

```yaml
# workflows/my-command.yaml
name: my-command
description: What this command does
arguments:
  - name: input
    type: string
    required: true
    description: Input description
steps:
  - run: python3 scripts/my_script.py
    args: [--input, "{{ args.input }}"]
```

### Gating Rules

OpenClaw filters skills at load time using metadata:

- **bins**: Required binaries on PATH
- **anyBins**: At least one binary required
- **env**: Environment variables required
- **config**: Config paths that must be truthy
- **os**: Platform restrictions
- **always**: Skip other gates

---

## Dual Compatibility Guidelines

### Do's

1. ✅ Always use YAML frontmatter
2. ✅ Use kebab-case for names
3. ✅ Keep descriptions under 1024 chars
4. ✅ Add OpenClaw metadata for gating if needed
5. ✅ Use `references/` for detailed docs

### Don'ts

1. ❌ Don't use angle brackets in description
2. ❌ Don't use `prompts/` (not in Agent Skills spec)
3. ❌ Don't start/end name with hyphen
4. ❌ Don't exceed name length limits

### Platform Detection

- **Agent Skills**: Uses `description` for matching, ignores `workflows/` and `prompts/`
- **OpenClaw**: Uses both YAML frontmatter + commands in `workflows/`

---

## Validation Comparison

| Rule | Agent Skills | OpenClaw |
|------|-------------|----------|
| Name max length | 64 | 64 |
| Description max | 1024 | 1024 |
| Compatibility max | 500 | - |
| Hyphen rules | No start/end, no double | Same |
| Directory match | Must match | Must match |
| i18n support | Yes (NFKC) | Same |
