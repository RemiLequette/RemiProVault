# Working with Forge

*Document type: Guide*

## Quick Start

Practical reference for using the Forge MCP tools correctly.
Covers when to use Forge, path format, the read → write decision, and common patterns.
Format details (sections, structured vs native) are returned by `forge_get_format` — no need to read the spec.

## Keywords
forge, MCP, path, absolute, windows, read, write, native, structured, patterns, workflow

## Table of Contents

1. [Why use Forge](#why-use-forge)
2. [Path format](#path-format)
3. [Read then write](#read-then-write)
4. [Common patterns](#common-patterns)
5. [Index](#index)

## Why use Forge
[up](#table-of-contents)

Forge is the standard file access layer for all projects using this KB. Use it instead of `filesystem` for all project file operations.

Two advantages over raw file access:

**Read narrow** — read one section of a document without loading the whole file. Large sections (changelogs, history) are lazy by default — Forge returns a key list; you load only what you need.

**Write narrow** — send only what changed. Forge updates the target section and leaves the rest untouched, with an automatic backup and rollback on error.

For unstructured files (`.js`, plain `.md` with no Forge metadata), Forge falls back to native mode: full read, full write — same as filesystem but within the same access layer.

## Path format
[up](#table-of-contents)

All Forge tools use **absolute Windows paths**. This applies to files and folders alike — `forge_ls`, `forge_mkdir`, `forge_rmdir`, `forge_move`, `forge_rename`, `forge_read`, `forge_create_*`, `forge_write_*`, `forge_delete`.

**Folders must end with `\`**. Files do not.

To discover allowed paths, call `forge_ls()` with no argument — it returns the list of allowed root directories:

```
forge_ls()
→ { allowedDirectories: ["C:\\Users\\RemiLequette\\Development\\knowledgebase", ...] }
```

Then navigate from there:

```
forge_ls("C:\\Users\\RemiLequette\\Development\\knowledgebase\\public\\")
→ { entries: [{ name: "conventions\\", type: "folder", path: "...\\conventions\\" }, ...] }
```

## Read then write
[up](#table-of-contents)

`forge_read` returns `isStructured` and `isNative` on every response. Use them to choose the write endpoint — do not guess from the extension.

```
forge_read(path)
  → isStructured: true   → forge_write_structured  /  forge_create_structured
  → isNative: true       → forge_write_native       /  forge_create_native
```

`forge_write_*` requires the file to exist. `forge_create_*` requires it not to exist.

If the format is unknown before reading (e.g. before `forge_create_structured`), call `forge_get_format` first — it lists all available structured formats with their section layout and intent.

## Common patterns
[up](#table-of-contents)

### Read a KB document

```
forge_read("C:\\Users\\RemiLequette\\Development\\knowledgebase\\public\\conventions\\documentation.md")
```

Lazy sections (changelog, history) return key lists only. Load a specific entry:

```
forge_read(path, "changelog.2026-06-18")
```

### Read multiple files in one call

```
forge_read({ paths: [
  "C:\\...\\public\\conventions\\forge.md",
  "C:\\...\\public\\conventions\\documentation.md"
] })
```

Results are returned in input order. A failing read does not abort the batch.

### Create a native file

```
forge_create_native("C:\\...\\knowledgebase\\tmp\\notes.md", "# Notes\n")
forge_write_native("C:\\...\\knowledgebase\\tmp\\notes.md", "# Notes\n\nUpdated content.")
```

### Create a structured file

```
forge_get_format()                    // discover available structured formats
forge_create_structured("C:\\...\\knowledgebase\\tmp\\log.md", "journal")
```

### Update a section (structured)

Send only the changed section — the rest is untouched:

```
forge_write_structured(path, {
  "header": { "content": "Updated header text." }
})
```

### Insert a changelog entry (structured)

```
forge_write_structured(path, {
  "changelog": {
    "_action": "insert",
    "_position": 0,
    "_value": { "title": "My change", "date": "2026-06-18", "content": "What changed." }
  }
})
```

`_position: 0` inserts first. Omit `_position` to append.

## Index

| Term | Occurrences |
|------|-------------|

## Changelog

### Version 1.2 - Why use Forge section added
**Date:** 2026-06-18
**Reason:** Guide started with path format — no explanation of what Forge is for or when to use it instead of filesystem.

**Changes:**
- Why use Forge: new first section — read narrow, write narrow, native fallback, replaces filesystem

---

### Version 1.1 - Path format corrected
**Date:** 2026-06-18
**Reason:** v1.0 described a `<rootName>/path` format that does not exist — all Forge tools use absolute Windows paths. forge_ls() returns allowedDirectories, not roots with URL. Removed reference to forge.md convention.

**Changes:**
- Quick Start: reference to convention removed
- Path format: rewritten — absolute Windows paths, folders end with `\`, forge_ls() discovery
- Common patterns: all paths corrected to absolute Windows format

---

### Version 1.0 - Creation
**Date:** 2026-06-18
**Reason:** Guide supprimé en session 2026-06-14 (décision : describes suffisent). Recréé après constat que les describes ne couvrent pas le flux de décision global ni le format des paths. Objectif : guide minimal, ne charge pas la convention.

**Changes:**
- Path format : syntaxe `<rootName>/path`, découverte via `forge_ls()`
- Read then write : décision `isStructured`/`isNative`, règle create vs write
- Common patterns : lecture KB, batch, create native, create structured, update section, insert changelog
