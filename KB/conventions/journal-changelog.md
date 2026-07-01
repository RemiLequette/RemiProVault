# Journal and Changelog Convention

Convention for session journals and centralized changelogs in any project.

*Document type: Convention*

## Quick Start

Covers two append-only log files maintained at the project root.
`Journal.md` records what happened during each working session.
`CHANGELOG.md` records what changed in each project file.
Load when creating or writing to either file.
Does not cover TODO or backlog — see `conventions/todo-list.md`.

## Load when
Writing a Journal entry or a Changelog entry, migrating inline changelogs


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

- Journal and Changelog Convention
    - Quick Start
    - Keywords
    - Journal
        - Why
        - File
        - Format
        - Rules
    - Changelog
        - Why
        - File
        - Format
        - Rules
```

## Journal

### Why

A session produces decisions, findings, and context that exist only in the chat. The Journal persists what matters across sessions without requiring the AI to reload entire project files. It is the narrative complement to the TODO — the TODO tracks what to do, the Journal records what was done and why.

### File

- Name: `Journal.md` at the project root
- Type: Log — exempt from `conventions/documentation.md` (no TOC, Keywords, Index, or Changelog required)
- One file per project, no monthly split

### Format

Entries are appended at the bottom of the file — most recent last. Each entry is a `##` heading with the date and a short session title, followed by free-form content.

```markdown
# Journal

## YYYY-MM-DD — Session title

Free-form content. Decisions made, findings, what was done, what was left open.
```

The session title should match or summarize the chat title — it is the primary navigation anchor for the file.

### Rules

- **Append only** — new entries go at the bottom. Existing entries are never modified.
- **One entry per session** — a session that produces nothing worth recording may be skipped.
- **AI Assistant** — writes only the current session entry at closure. Never rewrites or edits prior entries. Proposes the entry content; the human approves before it is written.

---

## Changelog

### Why

Tracking file history inside the file itself mixes content with history, increases file size, and costs tokens on every read. A centralized `CHANGELOG.md` separates history from content, keeps individual files lighter, and makes the evolution of the project readable in one place.

### File

- Name: `CHANGELOG.md` at the project root
- Type: Log — exempt from `conventions/documentation.md`
- One file per project

### Format

Entries are appended at the bottom — most recent last. Each entry targets one file and one change. If a session modifies several files, write one entry per file.

```markdown
# Changelog

## YYYY-MM-DD — path/to/file.md — Short change title

**Reason:** Why this change was made.

**Changes:**
- What changed, one item per line.
```

The file path is relative to the project root.

When a file changes so significantly that its history becomes irrelevant (full rewrite, format change), archive the old file before replacing it. The Changelog entry documents the archiving decision.

### Rules

- **Append only** — new entries go at the bottom. Existing entries are never modified.
- **One entry per file per session** — if the same file is modified twice in one session, merge into one entry.
- **No versioning** — date and file path are the only identifiers. Version numbers are not used.
- **AI Assistant** — writes entries at session closure, one per modified file. Proposes content; the human approves before writing.

