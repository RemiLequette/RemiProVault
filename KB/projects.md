# Projects

Directory of all projects using the Knowledge Base.

*Document type: Reference*

## Quick Start

Lists all active and archived projects that share this Knowledge Base.
Each entry provides enough context to understand a project's purpose, location, and dependencies without loading its files.
Update this file when registering a new project, archiving one, or when dependencies change.
See `conventions/project-registry.md` for the entry format and registration process.

## Keywords
projects, directory, registry, dependencies, active, archived

## Table of Contents

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

- Projects
    - Quick Start
    - Keywords
    - Table of Contents
    - Projects
        - KB Maintenance
        - GuideIA
        - DDScope
    - I
```

## Projects

### KB Maintenance

- **Tag:** `kb-maintenance`
- **Path:** C:\Users\RemiLequette\Development\knowledgebase
- **Status:** active
- **Description:** Maintenance project for the shared Knowledge Base — adds and updates conventions, guides, and tools in public/.
- **Uses KB conventions:** all
- **Depends on:** (none)

### GuideIA

- **Tag:** `guide-ia`
- **Path:** C:\Users\RemiLequette\Development\with-ai\guideIA
- **Status:** active
- **Description:** Guide pratique décrivant le fonctionnement interne d'un assistant IA et les bonnes pratiques pour l'utiliser efficacement — rédigé en français, produit via une collaboration humain-IA documentée.
- **Uses KB conventions:** documentation.md, documentation-style.md, todo-list.md, filesystem.md, tools.md
- **Depends on:** (none)

### DDScope

- **Tag:** `ddscope`
- **Path:** C:\Users\RemiLequette\Development\with-ai\ddscope
- **Status:** active
- **Description:** CommWise web application supporting DDMRP scoping workshops — supply chain mapping tool used by consultants for current-state capture before buffer design.
- **Uses KB conventions:** documentation.md, documentation-style.md, todo-list.md, journal-changelog.md, obsidian-links.md, idea-inbox.md
- **Depends on:** (none)
- **Note:** Project knowledge lives in its own Obsidian vault, separate from the KB/GuideIA vault — see `conventions/obsidian-links.md` for cross-vault reference rules.

