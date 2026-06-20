# Changelog

## 2026-06-20 — conventions/todo-list.md — Full rewrite v3.0 — Obsidian Bases architecture

**Reason:** Replaced the TODO.md flat-file approach with a folder-based structure using Obsidian Bases and one file per item, reducing AI Assistant read/write cost and enabling progressive disclosure.

**Changes:**
- Files: replaced TODO.md / TODO-archive.md / TODO-work.json with TODO/ folder structure (ITEMS/, ITEM template.md, TodoList.base)
- Features: removed card index, archiving, tool HTML; added Status/Importance/Effort as YAML front matter
- Format: replaced markdown checklist format with YAML front matter per item file
- AI Assistant: rewritten — list_directory for overview, targeted file read/write, board ignored

## 2026-06-20 — conventions/todo-list.md — Added Tools section (todo-filter.js)

**Reason:** AI Assistant needs a way to filter items by YAML property (e.g. WIP at session start) without reading all item files individually.

**Changes:**
- TOC: Tools entry added at position 5
- New section Tools: documents todo-filter.js — args, output, examples
- AI Assistant: added Filtered view bullet referencing todo-filter.js
