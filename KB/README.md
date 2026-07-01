# Knowledge Base

## Quick Start


Welcome to your centralized knowledge repository for managing  projects.

## Load when
Rarely by an AI Assistant — this file targets human readers. AI Assistants use `PROJECT.md` and the `kb-doc-index` MCP server instead.

```insta-toc
---
title:
  name:
  level:
  center:
exclude:
style:
  listType:
omit:
levels:
  min:
  max:
---

# Table of Contents

- Knowledge Base
    - Quick Start
    - Load when
    - Quick Navigation
    - Structure
    - What This Is
    - Key Files
    - For Humans & AI
    - Next Steps
```

## Quick Navigation

**New to this project?**
- Start here: `PROJECT.md`

**What are the design principles?**
- Read: `guides/best-practices.md`

**Auditing a project for conformance?**
- Read: `guides/audit-process.md`

## Structure

```
knowledgebase/
├── PROJECT.md           ← Bootstrap + project metadata (AI entry point)
├── conventions/         ← Technical standards & patterns
├── guides/              ← Setup & best practices documentation
├── tools/               ← Development tooling
├── README.md            ← This file
├── GLOSSARY.md          ← Project terminology
├── TODO/                ← Active backlog
├── howto/               ← Practical guides for humans
└── tmp/                 ← Temporary working files
```

## What This Is

A **reusable knowledge base** for structuring  projects with:
- **Conventions** — How to use tools, patterns, and standards
- **Guides** — Best practices and setup instructions
- **Tools** — Development utilities

Each new Claude project references this knowledge base, ensuring consistency across all projects.

## Key Files


| File | Purpose |
|------|--------|
| `PROJECT.md` | Bootstrap + metadata — session entry point for AI Assistants |
| `guides/best-practices.md` | 15 design principles for Claude projects |
| `guides/audit-process.md` | How to audit a project |

## For Humans & AI

This repository works for:
- **AI Assistants** — Bootstrap via `PROJECT.md` (loaded from Claude project instructions), decision layer via the `kb-doc-index` MCP server
- **You** — Start with `README.md` (this file), browse `howto/` for practical guides

## Next Steps

1. **Explore:** Browse `conventions/` and `guides/` to see what's available
2. **Learn:** Read the best practices in `guides/best-practices.md`
3. **Audit:** Use `guides/audit-process.md` to audit a project

