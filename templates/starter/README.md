---
name: starter
description: Basic agent template with provider, identity, and personality configuration
license: Apache-2.0
---

# Starter Template

A minimal agent template that provides the essential files to get started with a deepfactory-code agent.

## Included Files

- `agent.yaml` — Agent manifest with provider, memory, toolset, and policy references
- `IDENTITY.md` — Agent identity definition (customize with your agent's role and capabilities)
- `SOUL.md` — Agent personality and system prompt (customize with your agent's behavior)

## Usage

```bash
agent-builder template install starter
```

After installation, customize the files in `agents/<name>/` to fit your use case.
