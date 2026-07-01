# Project Knowledge Base Maintenance

## Quick Start

Project dedicated to the maintenance of the Knowledge Base shared by all AI Assited projects.

## Load when
Always — this is the KB's own project file, loaded unconditionally at the start of every session as part of the Bootstrap Sequence, not gated by the decision layer.

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

- Project Knowledge Base Maintenance
    - Quick Start
    - Load when
    - Purpose
    - Structure
        - Root — KB content, accessible to all projects
        - Project files — specific to this KB project
        - Tests
    - KB Maintenance
    - Decisions projet
    - Glossary
    - Audit
```

## Purpose
Centralized knowledge repository shared across all projects.
Contains conventions, workflows, and best practices to ensure consistency.

## Structure

### Root — KB content, accessible to all projects
Decision layer exposed live by the `kb-doc-index` MCP server — call `list_triggers(repo=kb)` for the full navigation map.

```
conventions/     ← technical conventions
guides/          ← setup & best practices guides
tools/           ← development tooling
```

### Project files — specific to this KB project
- **PROJECT.md** — this file
- **README.md** — human navigation
- **GLOSSARY.md** — project terminology
- **TODO/ — active backlog
- **Journal.md** — session log
- **howto/** — practical guides for humans
- **tests/** — test suites (private, not part of public KB)
- **tmp/** — temporary working files (not committed)

### Tests
Do not launch tests yourself, ask the user to launch with VSCode vitest extension

## KB Maintenance
**When to add content:**
- A tool behaves unexpectedly and a workaround is found — document it.
- A pattern proves reliable across multiple sessions — promote it.
- The designer explicitly asks to remember something technical — it belongs here, not in ephemeral memory.
- An innovation improves quality or reduces token cost noticeably — capture it.

**How to add:**
- Search via `kb-doc-index` (`search`, `list_triggers`) first — the convention may already exist or belong in an existing file.
- Create a new file in `conventions/` if the topic is distinct enough to stand alone.
- New files are picked up automatically by `kb-doc-index` (lazy reindex on next query, or `reindex` for a full sweep) — no manual navigation file to update.

**Format:** English only. Concise. Actionable. Prefer rules and examples over prose explanations.

**Guard rail:** Never propose changes to KB files unless the current project is the KB itself. In other projects, propose to document a finding instead.


## Decisions projet

- Les conventions (`conventions/`) et best practices (`guides/best-practices.md`) de ce projet sont **universelles** — elles s'appliquent à tous les projets, pas uniquement à cette KB.
- Chaque convention devrait avoir une best practice associée quand ça a du sens — la BP pointe vers la convention, elle ne duplique pas son contenu.

## Glossary

See `GLOSSARY.md` at project root.

## Audit

To audit this project's conformance to best practices:

1. Read: `guides/audit-process.md` — The audit methodology
2. Verify against: `guides/best-practices.md` — The best practices

See `guides/audit-process.md` for the complete process.

