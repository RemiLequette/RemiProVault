# Journal and Changelog Convention

Convention for session journals and centralized changelogs in any project.

*Document type: Convention*

## Quick Start

Covers two append-only log files maintained at the project root.
`Journal.md` records what happened during each working session.
`CHANGELOG.md` records what changed in each project file, replacing inline `## Changelog` sections.
Load when creating or writing to either file, or when migrating inline changelogs to the centralized format.
Does not cover TODO or backlog — see `conventions/todo-list.md`.

## Keywords
journal, changelog, log, session, append-only, traceability, migration

## Table of Contents

1. [Journal](#journal)
2. [Changelog](#changelog)
3. [Migration](#migration)
4. [Index](#index)

## Journal
[up](#table-of-contents)

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
[up](#table-of-contents)

### Why

Inline `## Changelog` sections in individual files mix content with history, increase file size, and cost tokens on every read. A centralized `CHANGELOG.md` separates history from content, keeps individual files lighter, and makes the evolution of the project readable in one place.

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

---

## Migration
[up](#table-of-contents)

### Why

Existing projects have inline `## Changelog` sections in individual files. Migration moves this history to `CHANGELOG.md` and removes the sections from the source files. After migration, `conventions/documentation.md` no longer requires `## Changelog` in individual files.

### Process

For each file with an inline `## Changelog`:

1. Convert each changelog entry to the centralized format — prefix the heading with the file path.
2. Append the converted entries to `CHANGELOG.md`, oldest first.
3. Remove the `## Changelog` section from the source file.
4. If the source file has a TOC, remove the Changelog entry from it.

This is a mechanical operation — no content is lost, only reorganized.

### Note on this KB

The KB files currently have inline `## Changelog` sections. Migration is a separate session — this convention is the prerequisite.

---

## Index

| Term | Occurrences |
|------|-------------|
