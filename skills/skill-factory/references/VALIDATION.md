# Validation Rules Reference

Complete validation rules extracted from both specifications.

## Name Validation

### Rules

| Rule | Constraint | Error Message |
|------|------------|---------------|
| Length | ≤ 64 chars | "exceeds 64 character limit" |
| Case | lowercase only | "must be lowercase" |
| Start | cannot be `-` | "cannot start with hyphen" |
| End | cannot be `-` | "cannot end with hyphen" |
| Consecutive | no `--` | "cannot contain consecutive hyphens" |
| Characters | alphanumeric + `-` only | "contains invalid characters" |
| Directory | must match name | "must match skill name" |

### i18n Support

Names support Unicode letters (normalized via NFKC):
- ✅ `中文-skills`
- ✅ `mon-skill`
- ❌ `UPPERCASE`
- ❌ `mixed_Case`

## Description Validation

| Rule | Constraint | Error |
|------|------------|-------|
| Required | non-empty string | "must be a non-empty string" |
| Length | ≤ 1024 chars | "exceeds 1024 character limit" |
| Characters | no `<` or `>` | "cannot contain angle brackets" |

## Frontmatter Fields

### Allowed Fields

```
name, description, license, compatibility, allowed-tools, metadata
```

### OpenClaw Extensions

Additional fields in `metadata.openclaw`:
- `always`, `emoji`, `homepage`, `os`
- `requires.bins`, `requires.anyBins`, `requires.env`, `requires.config`
- `primaryEnv`, `install`

## Validation Scripts

### Agent Skills (skills-ref)

```python
# From skills-ref/validator.py
ALLOWED_FIELDS = {
    "name",
    "description",
    "license",
    "allowed-tools",
    "metadata",
    "compatibility",
}

MAX_SKILL_NAME_LENGTH = 64
MAX_DESCRIPTION_LENGTH = 1024
MAX_COMPATIBILITY_LENGTH = 500
```

### OpenClaw (quick_validate.py)

```python
allowed_properties = {
    "name", "description", "license", 
    "allowed-tools", "metadata"
}
```

## Quick Validate Commands

```bash
# Agent Skills
skills-ref validate /path/to/skill

# OpenClaw
python3 quick_validate.py /path/to/skill
```

## Common Errors

1. **"Missing required field: name"** - Add name to frontmatter
2. **"Missing required field: description"** - Add description
3. **"Name must be lowercase"** - Convert to kebab-case
4. **"Directory name must match skill name"** - Rename folder
5. **"Description cannot contain angle brackets"** - Remove `<` or `>`
