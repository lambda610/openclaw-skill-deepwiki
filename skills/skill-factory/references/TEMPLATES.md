# Ready-to-Use Templates

Copy and customize these templates for your skills.

## Template 1: Minimal Agent Skills

```yaml
---
name: my-skill
description: Brief description of what this skill does. Use when user wants to [task].
---

# My Skill

## Overview

[What this enables]

## Usage

[How to use]
```

## Template 2: Full OpenClaw Skill

```yaml
---
name: my-skill
description: Comprehensive description. Use when: (1) Task A, (2) Task B, (3) Task C.
metadata:
  {
    "openclaw": {
      "requires": { "bins": [], "env": [], "config": [] },
      "emoji": "🎯"
    }
  }
---

# My Skill

## Overview

[What this enables]

## Commands

- `/my-skill command1` - Description
- `/my-skill command2` - Description

## Quick Start

[Basic usage examples]

## Scripts

- `scripts/script1.py` - What it does

## Reference

See [references/](references/) for detailed documentation.
```

## Template 3: Workflow File

```yaml
name: my-command
description: What this command does
arguments:
  - name: arg1
    type: string
    required: true
    description: Argument description
  - name: arg2
    type: boolean
    required: false
    description: Optional flag
    default: false
steps:
  - run: python3 scripts/my_script.py
    args: ["--arg1", "{{ args.arg1 }}"]
```

## Template 4: Reference Document

```markdown
# Skill Name - Reference

## API

### functionName

Description of function.

**Parameters:**
- `param1` (string): Description
- `param2` (number): Description

**Returns:** Description

**Example:**
```python
result = functionName(param1="value")
```

## Troubleshooting

### Common Issues

1. **Issue 1**: Solution
2. **Issue 2**: Solution
```

## Template 5: Script Boilerplate

```python
#!/usr/bin/env python3
"""
Skill script template

Usage:
    python3 script.py --input <value>
"""

import argparse
import sys


def main():
    parser = argparse.ArgumentParser(description="Description")
    parser.add_argument("--input", required=True, help="Input description")
    args = parser.parse_args()

    # Your logic here
    print(f"Processing: {args.input}")


if __name__ == "__main__":
    main()
```

## Template 6: Asset Placeholder

```markdown
# Asset Placeholder

This directory contains:

- `template.ext` - Template file
- `config.example.json` - Example configuration

Replace with actual files as needed.
```

## Quick Start Checklist

- [ ] Create skill directory
- [ ] Write SKILL.md with valid frontmatter
- [ ] Add scripts/ if needed
- [ ] Add references/ if needed
- [ ] Add workflows/ (OpenClaw only) if needed
- [ ] Validate with `skills-ref validate`
- [ ] Test in target platform
- [ ] Package for distribution
