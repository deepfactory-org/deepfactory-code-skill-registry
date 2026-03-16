---
name: update-docs
description: >
  Updates the agentcd documentation site (apps/web/docs — Astro Starlight) to stay in sync with
  codebase changes. Use this skill whenever you've just made changes to CLI flags/commands, API
  endpoints, built-in tools, configuration options, manifest resource kinds, admin features, Docker
  Compose configs, providers, sandboxes, memory backends, or any other feature that belongs in the
  docs. The docs site is the single source of truth for the project — it must always reflect the
  current state of the code. Invoke this proactively after completing any feature work, even if the
  user didn't explicitly ask to "update docs".
---

# agentcd Docs Updater

The docs site at `apps/web/docs/` uses **Astro Starlight** with plain Markdown (`.md`) pages
plus one `.mdx` landing page. It is the single source of truth for the project. Your job is to
keep it perfectly in sync with whatever just changed in the codebase.

## Step 1 — Understand What Changed

Start from the code, not from memory. Use `git diff` and read the actual source files to understand
what is new, modified, or removed:

```bash
git diff HEAD --stat                      # broad overview
git diff HEAD -- <specific-file>          # deep dive on a file
```

If the user told you what changed, use that as your starting point — but still read the source to
confirm the details. Documentation should describe reality, not intention.

## Step 2 — Map Changes to Doc Files

| What Changed | Doc File |
|---|---|
| CLI flags / commands (any app) | Relevant CLI reference or mode page |
| API endpoints | `api/endpoints.md` |
| Built-in tools | `deepfactory-code/tools.md` |
| Configuration options | `deepfactory-code/configuration.md` |
| Manifest resource kinds | `agent-builder/manifests.md` |
| Admin dashboard features | `admin/` pages |
| LLM providers | `deepfactory-code/providers.md` |
| Sandbox backends | `deepfactory-code/sandboxes.md` |
| Memory backends | `deepfactory-code/memory.md` |
| Docker Compose configs | `deployment/docker-compose.md` |
| Deployment / environments | `deployment/environments.md` |
| Hooks or lifecycle events | `deepfactory-code/configuration.md` |
| Architecture changes | `architecture/` pages |
| New major feature/subsystem | Possibly a new page (see Step 4) |

When uncertain, check `apps/web/docs/CLAUDE.md` for the authoritative mapping.

## Step 3 — Read, Then Update

For each doc file that needs updating:

1. **Read the current file** — understand what's already there before touching anything.
2. **Find the gap** — what's missing, outdated, or inconsistent with the code?
3. **Update minimally** — change only what needs to change; preserve existing structure, tone,
   and links. Don't rewrite sections that are still accurate.
4. **Add new sections** following the conventions below.

### Writing Conventions

**Frontmatter** — every page requires this exact structure:
```yaml
---
title: Short Title (2-5 words)
description: One-sentence description for preview/SEO.
---
```

**Headings** — `##` for main sections, `###` for subsections. No bare `#` headings — the title
comes from frontmatter.

**Tables** — preferred for flags, options, comparisons, and any structured list of things:
```markdown
| Column A | Column B |
|----------|----------|
| Value    | Value    |
```

**Code blocks** — always specify the language:
````markdown
```bash
just dev-dfc
```
````

**Internal links** — relative paths, trailing slash:
```markdown
[Link text](/deepfactory-code/overview/)
```

**Typical page structure** for reference/feature pages:
- Opening paragraph explaining purpose
- `##` sections for each major concept or feature group
- Tables for structured data (flags, options, modes)
- Code examples showing real usage
- "## Next Steps" or "## See Also" with links to related pages

**Tone** — direct, technical, factual. No marketing language. Document what exists, not what
is planned. Avoid "will support", "coming soon", or aspirational language.

## Step 4 — Adding New Pages

Only create a new page when a feature is substantial enough to warrant one — not for every
small addition. When you do:

1. Create `<slug>.md` in the right subdirectory under `apps/web/docs/src/content/docs/`.
2. Use the frontmatter pattern above.
3. Follow the writing conventions.
4. **Register it in the sidebar** — open `apps/web/docs/astro.config.mjs`, find the right
   section, and add:
   ```js
   { label: 'Page Title', slug: 'section/filename' }
   ```
   The slug must match the file path relative to `src/content/docs/`, without the `.md` extension.
5. Link to the new page from the section's overview page.

## Step 5 — Validate

Before finishing:

- Check that every internal link you wrote or kept actually points to an existing file.
- If you added pages to the sidebar, confirm the slug matches the file path.
- Optionally run `just build-docs` to catch any build errors — worth doing for larger changes.

## What Good Documentation Looks Like

- **Accurate** — describes what the code does right now, verified from source.
- **Concrete** — real commands, real config keys, real YAML examples — not pseudocode.
- **Current** — no dangling references to removed flags or old behavior.
- **Consistent** — matches the structure and tone of surrounding pages.
- **Right-sized** — covers the key concepts without exhaustive detail on every edge case.
