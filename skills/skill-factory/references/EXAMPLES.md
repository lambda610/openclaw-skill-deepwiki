# Real-World Skill Examples

Complete examples of dual-compatible skills.

## Example 1: Basic Agent Skills Only

### Structure
```
hello-skill/
├── SKILL.md
└── references/
    └── guide.md
```

### SKILL.md
```yaml
---
name: hello-skill
description: A simple greeting skill. Use when user wants to receive a friendly greeting or test skill loading.
---

# Hello Skill

## Overview

This skill demonstrates the minimum required structure for a valid Agent Skills skill.

## Usage

The skill automatically triggers when users ask for greetings like:
- "Hello"
- "Hi there"
- "Say hi"

## Response

Returns a friendly greeting message.

## Reference

See [guide.md](references/guide.md) for advanced usage.
```

### references/guide.md
```markdown
# Hello Skill Guide

## Advanced Usage

This skill can be extended to:
- Personalized greetings
- Time-based messages
- Multi-language support
```

---

## Example 2: Full-Featured OpenClaw Skill

### Structure
```
pdf-tools/
├── SKILL.md
├── workflows/
│   ├── merge.yaml
│   ├── split.yaml
│   └── extract.yaml
└── scripts/
    ├── merge.py
    ├── split.py
    └── extract.py
```

### SKILL.md
```yaml
---
name: pdf-tools
description: Comprehensive PDF manipulation toolkit. Use for: (1) Merging multiple PDFs, (2) Splitting PDF into pages, (3) Extracting text/images, (4) Rotating pages. Requires: Python 3 with pypdf library.
metadata:
  {
    "openclaw": {
      "requires": { "bins": ["python3"], "env": [] },
      "emoji": "📄"
    }
  }
---

# PDF Tools

## Overview

A complete PDF manipulation toolkit for merging, splitting, and extracting content.

## Commands

- `/pdf-tools merge` - Merge multiple PDFs
- `/pdf-tools split` - Split PDF into individual pages
- `/pdf-tools extract` - Extract text or images

## Quick Start

```bash
# Merge PDFs
python3 scripts/merge.py file1.pdf file2.pdf -o output.pdf

# Split PDF
python3 scripts/split.py input.pdf -o ./pages/

# Extract text
python3 scripts/extract.py input.pdf --text -o output.txt
```

## Scripts

- `merge.py` - Merge multiple PDF files
- `split.py` - Split PDF into separate pages
- `extract.py` - Extract text or images from PDF
```

### workflows/merge.yaml
```yaml
name: merge
description: Merge multiple PDF files into one
arguments:
  - name: inputs
    type: array
    required: true
    description: List of PDF files to merge
  - name: output
    type: string
    required: false
    description: Output file name
    default: "merged.pdf"
steps:
  - run: python3 scripts/merge.py
    args: ["{{ args.inputs }}", "--output", "{{ args.output }}"]
```

---

## Example 3: Dual-Compatible with Gating

### Structure
```
github-manager/
├── SKILL.md
├── workflows/
│   ├── create-issue.yaml
│   └── create-pr.yaml
├── scripts/
│   └── github.py
└── references/
    └── api.md
```

### SKILL.md
```yaml
---
name: github-manager
description: GitHub repository management. Use for: (1) Creating issues, (2) Managing PRs, (3) Listing repositories, (4) GitHub Actions management. Requires GitHub CLI (gh).
metadata:
  {
    "openclaw": {
      "requires": { "bins": ["gh"], "env": ["GITHUB_TOKEN"] },
      "primaryEnv": "GITHUB_TOKEN",
      "emoji": "🐙"
    }
  }
---

# GitHub Manager

## Overview

Manage GitHub repositories, issues, and PRs using the GitHub CLI.

## Prerequisites

- GitHub CLI installed (`gh`)
- `GITHUB_TOKEN` environment variable set

## Commands

- `/github-manager create-issue` - Create a new issue
- `/github-manager create-pr` - Create a pull request

## Usage

```bash
# Create issue
gh issue create --title "Bug: Login fails" --body "Description..."

# List repos
gh repo list
```

## Reference

See [api.md](references/api.md) for full API documentation.
```

---

## Example 4: Skill with Assets

### Structure
```
report-generator/
├── SKILL.md
├── scripts/
│   └── generate.py
└── assets/
    └── template.html
```

### SKILL.md
```yaml
---
name: report-generator
description: Generate HTML reports from data. Use when: (1) Creating status reports, (2) Building dashboards, (3) Generating HTML summaries.
---

# Report Generator

## Overview

Generate HTML reports from JSON or CSV data using customizable templates.

## Usage

```bash
python3 scripts/generate.py --template assets/template.html --data data.json --output report.html
```

## Templates

Custom templates can be placed in `assets/` directory.
```

---

## Migration Checklist

### Converting to Dual-Compatible

1. ✅ Add valid YAML frontmatter
2. ✅ Ensure name is kebab-case
3. ✅ Write clear description (< 1024 chars)
4. ✅ Remove angle brackets from description
5. ✅ Add OpenClaw metadata if needed
6. ✅ Add `workflows/` for OpenClaw commands
7. ✅ Move detailed docs to `references/`
8. ✅ Test with `skills-ref validate`
9. ✅ Test with OpenClaw gateway

### Testing Commands

```bash
# Agent Skills validation
skills-ref validate ./my-skill

# OpenClaw validation
python3 quick_validate.py ./my-skill

# Package for distribution
python3 package_skill.py ./my-skill
```
