# Knowledge Base

## Quick Start

Navigation humaine pour le projet knowledge base (KB)
Point d'entrée pour les humains : liens vers les fichiers clés, architecture du projet, description.
Pour un AI Assistant, lire `PROJECT.md` à la place.

Welcome to your centralized knowledge repository for managing Claude projects systematically.

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
    - Architecture
    - Structure
    - What This Is
    - Key Files
    - For Humans & AI
    - Next Steps
    - Keywords
```

## Quick Navigation

**New to this project?**
- Start here: `PROJECT.md`

**Setting up a new Claude project?**
- Read: `guides/project-setup-process.md`

**What are the design principles?**
- Read: `guides/best-practices.md`

**Auditing a project for conformance?**
- Read: `guides/audit-process.md`

**Practical how-to guides (for you)?**
- Browse: `howto/`

## Architecture

This knowledge base follows a **reference-based architecture**:

```
┌──────────────────────────────────────────────────┐
│         PROCESSES (Niveau Application)           │
│  ┌──────────────────┐    ┌──────────────────┐   │
│  │ audit-process    │    │ project-setup-   │   │
│  │                  │    │ process          │   │
│  │ Vérifier que le  │    │ Créer un nouveau │   │
│  │ projet respecte  │    │ projet respectant│   │
│  │ les practices    │    │ les practices    │   │
│  └────────┬─────────┘    └────────┬─────────┘   │
└───────────┼──────────────────────┼──────────────┘
            │                      │
            └──────────┬───────────┘
                       ↓
┌──────────────────────────────────────────────────┐
│        REFERENCE (Fondation)                     │
│     best-practices.md                            │
│     15 Design Principles                         │
└──────────────────────────────────────────────────┘
```

**How it works:**
- **Best Practices** are the single source of truth
- **Audit Process** verifies projects conform to best practices
- **Setup Process** creates new projects that follow best practices
- Both processes reference only the best practices — no duplication

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

A **reusable knowledge base** for structuring Claude projects with:
- **Conventions** — How to use tools, patterns, and standards
- **Guides** — Best practices and setup instructions
- **Tools** — Development utilities

Each new Claude project references this knowledge base, ensuring consistency across all projects.

## Key Files

| File | Purpose |
|------|--------|
| `PROJECT.md` | Bootstrap + metadata — session entry point for AI Assistants |
| `guides/project-setup-process.md` | How to create a new Claude project |
| `guides/best-practices.md` | 15 design principles for Claude projects |
| `guides/audit-process.md` | How to audit a project |

## For Humans & AI

This repository works for:
- **AI Assistants** — Bootstrap via `PROJECT.md` (loaded from Claude project instructions), decision layer via the `kb-doc-index` MCP server
- **You** — Start with `README.md` (this file), browse `howto/` for practical guides
- **Teams** — See `guides/project-setup-process.md` for setting up shared projects

## Next Steps

1. **Explore:** Browse `conventions/` and `guides/` to see what's available
2. **Setup:** Use `guides/project-setup-process.md` to create a new project
3. **Learn:** Read the best practices in `guides/best-practices.md`
4. **Audit:** Use `guides/audit-process.md` to audit a project


## Keywords
readme, navigation, knowledge-base, human, quickstart, architecture, structure, public
