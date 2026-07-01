# MCP Doc Index Convention

Architecture for an MCP server that indexes Markdown documents and exposes section-level read/write.

*Document type: Convention*

## Quick Start

Generic MCP server — not a KB-specific tool — that indexes `.md` documents conforming to `conventions/documentation.md` into a per-repo SQLite database with full-text search (FTS5), and exposes search plus section-level read/write to AI Assistants.
Load when working on the MCP server itself, its indexer, or its SQLite schema.
**Status: implemented.** Code lives under `KB-tools/mcp-doc-index/` (`server.js`, `tools.js`, `repos.json`) and `KB-tools/lib/` (`index-db.js`, `indexer.js`, `lock.js`, `repo-config.js`), with tests in `KB-tools/tests/`. Registered in the AI client as `kb-doc-index`.

## Load when
Working on the MCP doc index server, its indexer, or its SQLite schema

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

- MCP Doc Index Convention
    - Quick Start
    - Load when
    - Why
    - What — Model
        - Scope
        - Source of truth
        - Reindexing
        - Conformance
    - How — Implementation
        - File structure
        - SQLite schema
        - Client configuration
        - MCP tools
        - Design decisions
```

## Why

An AI Assistant working across many `.md` documents needs several things a flat folder of files doesn't provide: a way to find the right document and section for a given task without reading every file, a way to match a task to the documents relevant to it before being told explicitly which ones to read, and a way to edit a specific section without rewriting the whole document by hand. The MCP Doc Index provides a derived, queryable index — full-text search, task-to-document matching, and section-level read/write — generated automatically from conformant `.md` files, for the KB and for any project that opts in on its own documentation.

## What — Model

### Scope

Indexing scope is **configurable per repository** — the KB itself, and any project that opts in. Each indexed repo gets its own index; there is no single global index.

```
repos.json
[
  { "name": "kb", "root": "G:\\My Drive\\000-Projects\\100-RemiProVault\\KB", "db": "../data/kb.db" },
  { "name": "my-project", "root": "C:\\...\\my-project", "db": "../data/my-project.db" }
]
```

### Source of truth

SQLite is a **derived search index (FTS5)** — the `.md` files on disk remain the single source of truth. The database can be deleted and rebuilt from the repo's files at any time with no data loss.

### Reindexing

**Lazy reindex by mtime** — chosen over a filesystem watcher to avoid running a persistent daemon process. On every `search()` or `read_section()` call, the file's disk `mtime` is compared to `documents.mtime` in the database; if newer, that single file is reparsed and upserted before the call answers.

**Known limitation:** lazy reindex does not detect new or deleted files until they are searched for — it only refreshes files already known to the index. The `reindex(repo)` tool covers catch-up: first run on a new repo, or after bulk filesystem changes (renames, deletions, a manual cleanup session like the one that just happened on the KB root).

### Conformance

`write_section` is **blocking** on conformance: after applying the edit, the resulting document is checked with `md-parser.js [getIssues]`. If issues remain, the write is rejected and the file is left untouched — the caller must fix the conformance issue (e.g. a newly-emptied `## Keywords`) before the write can succeed.

## How — Implementation

### File structure

```
KB-tools/
  lib/
    md-parser.js        ← existing, reused as-is for parsing and section mutation
    fs-scan.js           ← existing, reused for repo directory walking
    indexer.js            ← NEW — scan(repo) -> upsert rows into SQLite
    index-db.js             ← NEW — DB open/schema/queries, one connection per repo db
  mcp-doc-index/
    server.js                ← MCP server entry point — exposes the tools below
    repos.json                  ← repo roots + db file mapping, see Scope above
  data/
    kb.db                        ← generated, gitignored
    <project-name>.db            ← one per indexed repo
```

The `data/` folder is at the KB-tools root, shared across all repos. Database paths in `repos.json` are relative to `server.js` (e.g. `../data/kb.db`). The folder and its contents are gitignored — `.db` files are never versioned, per `conventions/sqlite.md`.

### SQLite schema

```sql
CREATE TABLE documents (
  id        INTEGER PRIMARY KEY,
  repo      TEXT NOT NULL,
  file_path TEXT NOT NULL,
  title     TEXT,
  load_when TEXT,
  mtime     INTEGER NOT NULL,
  UNIQUE(repo, file_path)
);

CREATE TABLE sections (
  id        INTEGER PRIMARY KEY,
  doc_id    INTEGER NOT NULL REFERENCES documents(id) ON DELETE CASCADE,
  name      TEXT NOT NULL,
  content   TEXT NOT NULL
);

-- Non-content FTS5 table (not content= linked to sections) — rowid is set to
-- sections.id on insert so joins stay simple. doc_id is stored UNINDEXED so it
-- can be used to scope a search without taking part in full-text matching.
CREATE VIRTUAL TABLE sections_fts USING fts5(
  name, content, doc_id UNINDEXED
);
```

`documents.load_when` is sourced from the `## Load when` section of each document — one trigger phrase per line, parsed by `md-parser.js [getLoadWhen]`. Documents without `## Load when` have `load_when = NULL` and are matched on title and body only.

One `.db` file per repo (see Scope above) — never a single file mixing several repos. `schema.sql` is the only schema artifact tracked in Git; the `.db` files themselves are gitignored, per `conventions/sqlite.md [section Schema changes]`.

### Client configuration

The server is registered in the AI client under the name `kb-doc-index`. Example configuration (Claude and compatible clients):

```json
{
  "mcpServers": {
    "kb-doc-index": {
      "command": "node",
      "args": ["C:\\Users\\...\\KB-tools\\mcp-doc-index\\server.js"]
    }
  }
}
```

The path in `args` is the absolute path to `server.js` on the local machine.

### MCP tools

Implemented as a thin `server.js` (MCP transport wiring — `@modelcontextprotocol/sdk`'s `McpServer` + `StdioServerTransport`) over a pure `tools.js` orchestration layer, the latter unit-tested directly without going through the protocol.

| Tool | Args | Effect |
|------|------|--------|
| `search` | `query, repo?` | FTS5 query across `sections_fts` (+ `documents.title`/`load_when` via `searchDocumentMeta`). Returns `{ title_matches, section_matches }` — title/load_when hits are a separate, higher-relevance group ahead of section-body matches (see Design decisions). Refreshes (lazy reindex) every already-known document in scope before answering. `repo` omitted searches all configured repos. |
| `list_triggers` | `repo` | Returns the `load_when` → file rows for the repo as JSON (`file_path, title, load_when`), built from `documents.load_when`. The AI reads it in one call and matches the current task against it. |
| `read_section` | `repo, file, section` | Lazy reindex check, then delegates to `md-parser.js [getSection]`. |
| `write_section` | `repo, file, section, content?, mode?, position?` | `mode` defaults to `set`. Delegates to `md-parser.js [setSection / insertSectionAt / deleteSection]` per mode, then `toMarkdown`, runs `getIssues` — rejects on non-empty issues — writes the file, then reindexes that one file. Acquires the repo's advisory lock for the duration (see Design decisions). |
| `reindex` | `repo?` | Full directory walk via `fs-scan.js`, upserts all documents in the named repo (or all configured repos if omitted). Catches new/deleted files that lazy reindex misses. |

### Design decisions

**Section insertion/deletion.** Folded into `write_section`, not separate tools — one write entry point keeps the conformance gate (`getIssues`) as the single choke point for every mutation. The tool signature carries an explicit `mode`:

| `mode` | Behaviour |
|--------|-----------|
| `set` (default) | `setSection` — create or overwrite. |
| `insert` | `insertSectionAt(name, content, position)` — `position` required, same `before:<name>` / `after:<name>` / `beginning` syntax as `md-parser.js`. |
| `delete` | `deleteSection(name)` — `content` ignored if passed. |

All three modes go through the same path: mutate the in-memory doc → `toMarkdown` → `getIssues` → write file or reject. This is why a separate `insert_section` / `delete_section` tool would duplicate that gate for no benefit.

**Concurrency.** Single-writer, advisory file lock — sufficient for the actual usage pattern (one AI Assistant session per repo at a time; the KB's Scope Rule already forbids cross-project access, which keeps concurrent writers to the same repo rare). `write_section` acquires a lock file (`<db>.lock`, containing the writer's PID and timestamp) before read-modify-write, releases it after. A stale lock (PID no longer running, or older than a fixed timeout — 30s) is reclaimed automatically rather than blocking forever. No multi-writer merge strategy is designed because the scope does not require one; if cross-session concurrent writes become a real pattern, this is the section to revisit.

**Search ranking.** FTS5 defaults (`bm25`, descending) for ranking — no custom weighting. Snippet via FTS5's built-in `snippet()` function: ~64 tokens of context around the match, `<mark>`/`</mark>` around matched terms. `documents.title` and `documents.load_when` matches are surfaced as a separate result group ranked above section-body matches, since a title or trigger hit is a stronger signal of relevance than a body hit. Revisit only if FTS5 defaults prove inadequate in practice.
