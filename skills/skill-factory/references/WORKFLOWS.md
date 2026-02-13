# OpenClaw Workflow Patterns

Workflows define commands available in OpenClaw skills.

## Workflow Directory Structure

```
my-skill/
├── workflows/
│   ├── command1.yaml
│   └── command2.yaml
└── scripts/
    └── helper.py
```

## Basic Workflow Format

```yaml
name: my-command
description: What this command does
arguments:
  - name: arg_name
    type: string
    required: true
    description: Description of the argument
steps:
  - run: python3 scripts/helper.py
    args: [--arg, "{{ args.arg_name }}"]
```

## Argument Types

| Type | Description | Example |
|------|-------------|---------|
| string | Text value | `"hello"` |
| number | Numeric value | `42` |
| boolean | true/false | `true` |
| array | List of values | `["a", "b"]` |

## Workflow Examples

### Example 1: Simple Command

```yaml
name: greet
description: Send a greeting message
arguments:
  - name: name
    type: string
    required: true
    description: Name to greet
steps:
  - run: echo
    args: ["Hello, {{ args.name }}!"]
```

### Example 2: Multi-step Workflow

```yaml
name: process-file
description: Process a file with multiple steps
arguments:
  - name: input
    type: string
    required: true
    description: Input file path
  - name: output
    type: string
    required: false
    description: Output file path
steps:
  - run: python3 scripts/validate.py
    args: ["{{ args.input }}"]
  - run: python3 scripts/transform.py
    args: ["{{ args.input }}", "{{ args.output }}"]
  - run: python3 scripts/report.py
    args: ["{{ args.output }}"]
```

### Example 3: Conditional Execution

```yaml
name: deploy
description: Deploy to environment
arguments:
  - name: environment
    type: string
    required: true
    description: Target environment (dev/staging/prod)
    enum: [dev, staging, prod]
steps:
  - run: python3 scripts/check_env.py
    args: ["{{ args.environment }}"]
  - run: python3 scripts/deploy.py
    args: ["{{ args.environment }}"]
```

### Example 4: With Environment Variables

```yaml
name: api-call
description: Make an API call
arguments:
  - name: endpoint
    type: string
    required: true
env:
  API_KEY: "{{ secrets.api_key }}"
steps:
  - run: python3 scripts/api_client.py
    args: ["{{ args.endpoint }}"]
```

## Advanced Patterns

### Error Handling

```yaml
name: safe-deploy
description: Deploy with rollback on failure
arguments:
  - name: version
    type: string
    required: true
steps:
  - run: python3 scripts/deploy.py
    args: ["{{ args.version }}"]
    on-error:
      - run: python3 scripts/rollback.py
        args: ["{{ args.version }}"]
```

### File Operations

```yaml
name: convert-image
description: Convert image format
arguments:
  - name: input
    type: string
    required: true
  - name: format
    type: string
    required: true
    enum: [png, jpg, webp]
steps:
  - run: python3 scripts/convert.py
    args: ["{{ args.input }}", "--format", "{{ args.format }}"]
    env:
      OUTPUT_DIR: "./output"
```

## Command Dispatch

In SKILL.md frontmatter:

```yaml
---
name: image-processor
description: Process images
user-invocable: true
command-dispatch: tool
command-tool: image_processor
command-arg-mode: raw
---
```

This allows `/image-processor <args>` to dispatch directly to a tool.

## Best Practices

1. **Keep commands focused** - One command, one responsibility
2. **Use descriptive arguments** - Help users understand inputs
3. **Add validation** - Validate inputs before processing
4. **Provide defaults** - Make optional arguments when possible
5. **Document errors** - Clear error messages for failures
