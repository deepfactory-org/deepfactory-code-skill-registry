---
name: spec-writer
description: >
  Write structured technical specifications for AI agent features and implementation plans.
  Use this skill whenever the user asks to plan, design, architect, or spec out a feature, system,
  or change — especially when entering plan mode, using /plan, or requesting an implementation
  strategy. Also trigger when the user says things like "let's think through how to build X",
  "write a spec for Y", "design the approach for Z", or any variation of planning work before
  coding. The spec output is dual-purpose: readable by engineers AND optimized as input for AI
  agents that will implement it. Always use this skill for plan-mode outputs to ensure consistent,
  well-structured markdown specs saved to the /spec directory.
---

# Spec Writer

Write technical specifications that serve two audiences simultaneously: human engineers who need
to understand the design, and AI agents who will use the spec as implementation instructions.

## Why This Matters

Specs are the bridge between "what we want" and "what gets built." A good spec eliminates
back-and-forth, prevents wasted implementation effort, and gives AI coding agents the precise
context they need to produce correct code on the first pass. Structure and clarity aren't just
nice-to-have — they directly affect implementation quality.

## Step 1 — Understand the Request

Before writing anything, gather enough context to write a useful spec:

1. **Read existing code** — understand the current architecture, patterns, and conventions
   in the areas that will be affected. Use `git diff`, grep for related types/interfaces,
   and read the key files.
2. **Read existing specs** — check `spec/` for prior specs that might overlap or provide context.
3. **Clarify scope** — if the request is vague, ask focused questions. Prefer "what problem does
   this solve?" over "what do you want me to build?"
4. **Identify constraints** — runtime, language, compatibility, performance requirements.

## Step 2 — Choose the Right Structure

Every spec lives under `spec/` in the project root. Use folders and subfolders to keep things
navigable.

### Naming Convention

```
spec/
├── spec.md                          # Legacy/existing master spec (don't touch)
├── <feature-name>/
│   ├── README.md                    # Overview & navigation index
│   ├── 01-problem-statement.md      # What and why
│   ├── 02-architecture.md           # High-level design
│   ├── 03-implementation.md         # Detailed build plan
│   ├── 04-api-contracts.md          # Interfaces, types, endpoints (if applicable)
│   ├── 05-testing-strategy.md       # How to verify it works
│   └── subsystem/                   # Sub-specs for complex subsystems
│       ├── README.md
│       └── *.md
```

For **small features** (< 1 day of work), a single file is fine:
```
spec/<feature-name>.md
```

For **medium features**, use a folder with README + 2-3 focused files.

For **large features**, use the full folder structure with subsystem subfolders.

Use your judgment — the goal is that someone (human or AI) can find what they need quickly.

### Numbering

Prefix files with `01-`, `02-`, etc. so they sort in reading order. This helps both humans
browsing the directory and AI agents that process files sequentially.

## Step 3 — Write the Spec

### README.md (folder specs only)

The README is the entry point. It should contain:

```markdown
# <Feature Name>

> One-line summary of what this feature does.

## Status

`draft` | `review` | `approved` | `in-progress` | `done`

## Quick Links

- [Problem Statement](./01-problem-statement.md)
- [Architecture](./02-architecture.md)
- [Implementation Plan](./03-implementation.md)

## Overview

2-3 paragraphs explaining the feature at a high level. What it does, why it matters,
and how it fits into the existing system.

## Scope

### In Scope
- Bullet list of what this spec covers

### Out of Scope
- Bullet list of what it explicitly does NOT cover

## Key Decisions

| Decision | Choice | Rationale |
|----------|--------|-----------|
| ...      | ...    | ...       |
```

### Problem Statement (01-problem-statement.md)

```markdown
# Problem Statement

## Current State
Describe what exists today. Reference specific files and code paths.

## Pain Points
What's broken, missing, or suboptimal? Be concrete.

## Desired Outcome
What does success look like? Include measurable criteria where possible.

## User Stories (if applicable)
- As a [role], I want [capability] so that [benefit]
```

### Architecture (02-architecture.md)

```markdown
# Architecture

## Design Overview
High-level description of the approach. Use ASCII diagrams for data flow:

    ┌─────────┐     ┌──────────┐     ┌─────────┐
    │  Input   │────▶│ Process  │────▶│ Output  │
    └─────────┘     └──────────┘     └─────────┘

## Components

### Component Name
- **Purpose**: what it does
- **Location**: `path/to/package/`
- **Interfaces**: key types/interfaces it exposes
- **Dependencies**: what it depends on

## Data Flow
Step-by-step description of how data moves through the system.

## Alternatives Considered

| Approach | Pros | Cons | Verdict |
|----------|------|------|---------|
| ...      | ...  | ...  | ...     |
```

### Implementation Plan (03-implementation.md)

This is the most important file for AI agents. Be precise and actionable.

```markdown
# Implementation Plan

## Prerequisites
What must exist or be true before implementation starts.

## Build Sequence

Work items ordered by dependency. Each item should be independently testable.

### Phase 1: <Name>

#### 1.1 — <Task Title>
- **File**: `path/to/file.go` (create | modify)
- **What**: Concise description of the change
- **Details**:
  - Specific struct/interface/function to add or modify
  - Key logic or algorithm notes
  - Error handling expectations
- **Tests**: What to test and how
- **Done when**: Concrete acceptance criteria

#### 1.2 — <Next Task>
...

### Phase 2: <Name>
...

## File Manifest

Summary table of all files that will be created or modified:

| File | Action | Description |
|------|--------|-------------|
| `path/to/new.go` | Create | Brief description |
| `path/to/existing.go` | Modify | What changes |

## Migration / Rollback
If applicable, how to migrate data and how to roll back if things go wrong.
```

### API Contracts (04-api-contracts.md, if applicable)

```markdown
# API Contracts

## Types

\```go
type MyStruct struct {
    Field string `json:"field"`
}
\```

## Interfaces

\```go
type MyInterface interface {
    DoThing(ctx context.Context, input Input) (Output, error)
}
\```

## Endpoints (if REST/gRPC)

### POST /api/v1/resource
- **Request**: `CreateResourceRequest`
- **Response**: `Resource`
- **Errors**: 400 (validation), 404 (not found), 500 (internal)
```

### Testing Strategy (05-testing-strategy.md)

```markdown
# Testing Strategy

## Unit Tests
What to unit test and key test cases.

## Integration Tests
What to integration test, required fixtures/mocks.

## Manual Verification
Steps to manually verify the feature works end-to-end.
```

## Writing Guidelines

These guidelines make specs useful for both humans and AI agents:

### Be Concrete, Not Abstract
- Reference actual file paths, function names, and types from the codebase
- Use real examples, not pseudocode
- Include actual CLI commands to test things

### Use Tables for Structured Data
Tables are scannable for humans and parseable for AI:
```markdown
| Config Key | Type | Default | Description |
|------------|------|---------|-------------|
| `timeout`  | int  | 30      | Seconds     |
```

### Use Code Blocks with Language Tags
Always specify the language for syntax highlighting and AI parsing:
````markdown
```go
func Example() {}
```
````

### Keep Paragraphs Short
3-4 sentences max. Use bullet lists for related items. White space aids readability.

### Link to Source
When referencing existing code, include the path:
> The engine loop at `apps/deepfactory-code/internal/harness/engine/engine.go` handles...

### Mark Uncertainty
If something is tentative or needs discussion, mark it clearly:
> **[OPEN QUESTION]**: Should we use polling or webhooks here?
>
> **[TBD]**: Exact retry backoff strategy.

### Include Context for AI Agents
AI agents reading the spec need to understand:
- What already exists (so they don't recreate it)
- What patterns to follow (so code is consistent)
- What the acceptance criteria are (so they know when they're done)
- What order to build things (so dependencies are satisfied)

## Step 4 — Review Checklist

Before presenting the spec:

- [ ] Every file path referenced actually exists (or is clearly marked as "to create")
- [ ] The implementation plan has a clear build order with dependencies
- [ ] Scope boundaries are explicit (in-scope vs out-of-scope)
- [ ] No hand-wavy sections — if something isn't decided yet, mark it as [TBD]
- [ ] ASCII diagrams or tables used where they add clarity
- [ ] The spec is self-contained enough that an AI agent could implement from it alone
