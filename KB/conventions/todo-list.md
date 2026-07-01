# Todo List Convention

Convention for the lightweight backlog of any project — ideas, improvements, tasks.

## Quick Start

Defines how to structure and use a lightweight backlog in a project.
Load when creating or manipulating todo items, or when an AI Assistant needs to add or update items during a session.
Does not cover bug tracking, tests, or development tasks.

## Load when
Creating or managing a TODO or backlog


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

- Todo List Convention
    - Quick Start
    - Keywords
    - Scope
    - Features
    - Files
    - Format
        - ITEM template.md
        - Item file
    - Tools
        - todo-filter.js
    - AI Assistant
    - Index
```

## Scope

The todo list is a **lightweight backlog** — it captures ideas and tasks for a project.

**In scope:**
- Improvement ideas
- Tasks identified during a session or audit
- Features to explore

**Out of scope:**
- Bugs and technical fixes
- Development and test tasks
- Detailed project management

## Features

- **Capture** — create a new item at any time during a session
- **Status** — Idea / Todo / WIP / Done
- **Importance** — High / Medium / Low
- **Effort** — XS / S / M / L / XL
- **Description** — free-form content in the item file body
- **WIP** — items with `Status: WIP` represent what is currently in progress across sessions. The WIP is the bridge between sessions: a session closes by reviewing WIP items, the next session opens by reading them.

## Files

One folder per project at the project root:

```
TODO/
  ITEM template.md       — template for new items
  TodoList.base          — Obsidian Bases board (managed by user)
  ITEMS/
    Item title.md        — one file per todo item
```

- **`TODO/ITEMS/`** — contains all active todo items, one `.md` file per item
- **`TODO/ITEM template.md`** — template used to create new items
- **`TODO/TodoList.base`** — Obsidian Bases Kanban board; managed by the user, not read by the AI Assistant

There is no archive file. Done items remain in `TODO/ITEMS/` with `Status: Done`.

## Format

### ITEM template.md

```markdown
---
Status: Todo
importance: Medium
effort: M
type: todo
---

## Description

## Notes
```

### Item file

Each item is a `.md` file in `TODO/ITEMS/`. The filename is the item title.

```markdown
---
Status: Todo
importance: High
effort: S
type: todo
---

## Description

Optional free-form content.

## Notes
```

**Status values:** `Idea` / `Todo` / `WIP` / `Done`
**Importance values:** `High` / `Medium` / `Low`
**Effort values:** `XS` / `S` / `M` / `L` / `XL`

## Tools

### todo-filter.js

Filters todo items by YAML front matter property. Avoids reading all item files individually when only a subset is needed (e.g. WIP items at session start).

**Location:** `G:\My Drive\000-Projects\110-Projects\tools\todo-filter.js`
**Spec:** `conventions/tools.md`

**Args:** `<items-dir> <property> <value>`

- `<items-dir>` — absolute path to `TODO/ITEMS/`
- `<property>` — YAML property to filter on (`Status`, `importance`, `effort`)
- `<value>` — value to match (case-sensitive)

**Output (stdout):**
- `OK` — followed by one filename per line (matching items), empty if none
- `ERROR:<code>:<message>` — see `conventions/tools.md` for error codes

**Examples:**
```
node tools/todo-filter.js /path/to/TODO/ITEMS Status WIP
node tools/todo-filter.js /path/to/TODO/ITEMS importance High
node tools/todo-filter.js /path/to/TODO/ITEMS effort XS
```

## AI Assistant

- **Overview** — `list_directory` on `TODO/ITEMS/` gives the list of all items. Read individual files only when detail is needed.
- **Filtered view** — use `todo-filter.js` to get items matching a specific property value (e.g. `Status WIP` at session start).
- **Create an item** — copy `TODO/ITEM template.md` to `TODO/ITEMS/<title>.md`, fill in the YAML front matter.
- **Update an item** — write only the targeted file.
- **Change status** — update only the `Status` property in the YAML front matter.
- **Board** — `TODO/TodoList.base` is managed by the user. The AI Assistant does not read or modify it.

An AI Assistant may only create, modify, or delete items with **explicit user approval**.
