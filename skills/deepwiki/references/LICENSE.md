# License Documentation Guide

This guide helps generate appropriate license documentation for projects.

## When to Use

- Creating new open source projects
- Adding licenses to existing projects
- Choosing appropriate licenses

## Common License Types

### Permissive Licenses

| License | Key Features | Use Case |
|---------|--------------|----------|
| **MIT** | Simple, permissive | Most common for libraries |
| **Apache 2.0** | Patent grants included | Commercial-friendly |
| **BSD** | Similar to MIT | Legacy projects |

### Copyleft Licenses

| License | Key Features | Use Case |
|---------|--------------|----------|
| **GPL v3** | Strong copyleft | Ensure derivatives stay open |
| **LGPL** | Weak copyleft | Libraries |

## Output Template

```markdown
# [Project Name]

License: [License Name]

## Brief Description
[What this license means in plain terms]

## Permissions
- ✓ Commercial use
- ✓ Distribution
- ✓ Modification
- ✓ Private use

## Conditions
- [ ] Include license
- [ ] State changes (if applicable)

## Limitations
- No warranty
- No liability
```

## Tips

1. **Choose based on goals**: Commercial? Open derivatives?
2. **Include in**: README.md, LICENSE file, package.json
3. **Header comments**: Add to source files

## Output Location

```
LICENSE
README.md (license section)
```
