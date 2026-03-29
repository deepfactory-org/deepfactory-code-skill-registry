---
name: starter
description: Basic agent template with behavior configuration (identity, soul, instructions)
license: Apache-2.0
---

# Starter Agent

A minimal agent template that provides the essential files to get started with an AgentCD agent.

## Included Files

- `agent-manifest.yaml` — Agent manifest with behavior, capabilities, and defaults
- `IDENTITY.md` — Agent identity: role, expertise, responsibilities
- `SOUL.md` — Agent personality: tone, problem-solving approach, boundaries

## Behavior Configuration

Each section under `behavior` accepts either inline markdown or a file reference:

```yaml
behavior:
  identity:
    file: IDENTITY.md       # load from file
  soul:
    file: SOUL.md
  heartbeat:
    file: HEARTBEAT.md      # optional — autonomous scheduling
  instructions:
    inline: |               # or write directly in YAML
      - Be concise and reliable
```

## Usage

```bash
agentcd-builder template install starter
```

After installation, customize the behavior files to fit your use case.
