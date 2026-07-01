# Documentation Style Convention

Convention for document types and content structure — what kind of document this is, and how its content should be organized.

## Quick Start

Convention complementary to `conventions/documentation.md`, which governs form (structure, headings, TOC).
This convention governs content style: every document must declare its type, and documents describing a system, process, or decision must make their intent, model, and implementation explicitly separable.
Load when creating a new document or auditing documentation content quality.
Does not cover document structure rules — see `conventions/documentation.md`.

## Load when
Creating or editing a .md file
Declaring a document type, auditing content style


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

- Documentation Style Convention
    - Quick Start
    - Keywords
    - Why-What-How Hierarchy
        - Why this matters
        - Separation rule
        - Anti-patterns
    - Document Taxonomy
        - Convention
        - Process spec
        - Guide
        - Reference
        - Project file
        - Log
    - WWH by Document Type
```

## Table of Contents

1. [Load when](#load-when)
2. [Why-What-How Hierarchy](#why-what-how-hierarchy)
3. [Document Taxonomy](#document-taxonomy)
4. [WWH by Document Type](#wwh-by-document-type)

## Why-What-How Hierarchy
[up](#table-of-contents)

Documents describing a system, process, or decision must make three levels of content explicitly separable. This hierarchy is abbreviated **WWH**.

| Level | Question | Content |
|---|---|---|
| **Why** — Intent | Why does this exist? What problem does it solve? | Goals, motivations, constraints that justify the design |
| **What** — Model | What is the conceptual model? | States, entities, rules, lifecycle — independent of technology |
| **How** — Implementation | How is the model realized? | Technology, data formats, scripts, UI |

### Why this matters

Without the Why, an AI Assistant (or a new team member) cannot judge whether a proposed change is aligned with the intent. Rules followed without understanding their purpose get applied mechanically — and break silently when the context changes.

Without separating What from How, the model becomes coupled to its implementation. Changing the technology (replacing a JSON file with a database) requires understanding the entire document to find what is invariant.

**Example — meeting attendance:**

| Level | Content |
|---|---|
| Why | Track member attendance over time as a governance indicator for a volunteer association. Documented excuses distinguish justified absence from disengagement. |
| What | Each participant has a planned presence (set before the meeting) and an actual presence (recorded during). Valid states: `present`, `excuse`, `absent`. An excuse requires a documented source. |
| How | `presence_prevue` field in `data-YYYY-MM-DD.json`, editable in `prepare` mode. `presence_reelle` field in `changes-YYYY-MM-DD.json`, editable in `edit` mode. |

### Separation rule

The three levels must be **recognizable** in the document structure — not necessarily as three sections with these exact names, but a reader must be able to locate each level without reading the entire document.

**Recommended patterns:**

For a process spec:
```
## Objectives          ← Why
## Model / States      ← What
## Data schema         ← How (technical realization of the model)
## Scripts             ← How (automation)
```

For a convention:
```
## Quick Start         ← Why (why load this, what it covers)
## [Rules / Patterns]  ← What (the rules being defined)
## [Examples / Usage]  ← How (how to apply them)
```

### Anti-patterns

**Mixed levels ❌**
```
## Attendance
presence_prevue can be "present" or "excuse".
Set it before the meeting to track who plans to come.
The field is in data-YYYY-MM-DD.json under participants[].
```
Why (track attendance), What (two states, set before meeting), and How (JSON field name) are indistinguishable.

**Implementation without model ❌**
```
## Attendance
Set presence_prevue = "present" or "excuse" in data-*.json in prepare mode.
Set presence_reelle = "present", "excuse", or "absent" in changes-*.json in edit mode.
```
An AI reading this cannot determine whether `absent` is valid for `presence_prevue` — the model is not stated, only the implementation.

**Model without intent ❌**
```
## Attendance states
planned: present | excuse
actual: present | excuse | absent
```
Without the Why, an AI cannot evaluate whether adding a new state (e.g. `late`) is appropriate — it does not know what the model is trying to capture.

**Signalling what was replaced ❌**
```
Replaces the navigation role md-doc/TOC previously played for AI Assistants.
```
Mentioning a deprecated mechanism, an old role a section used to play, or what something "replaces" makes that obsolete thing more visible, not less — a reader (human or AI) notices the explicit warning and pays attention to exactly what they were told to ignore. State the current behavior only; let the absence of the old reference do the work.

## Document Taxonomy
[up](#table-of-contents)

Every document must declare its type. The type declaration format is governed by `conventions/documentation.md [section Scope]`.


### Convention

A convention leaves a trace in the artifacts — conformance is auditable by examining documents, code, or data directly.

**Examples:** `conventions/documentation.md`

**Style:** Precise and imperative. Rules are stated clearly, with a WHY when non-obvious. Anti-patterns illustrate what to avoid. Exceptions are explicitly listed.


### Process spec

A document that describes a repeatable process end-to-end: what it accomplishes, the conceptual model it relies on, and how it is technically implemented.

**Examples:** `reunions/gestion-reunions.md`, a deployment process, a review workflow.

**Style:** Layered — the three WWH levels must be explicitly separable. A reader must be able to understand the intent without reading the implementation, and the model without reading the technical details.


### Guide

A guide leaves a trace in the process — auditing whether it was followed requires a log.

**Examples:** `guides/audit-process.md`

**Style:** Action-oriented. Steps are numbered and executable. Prerequisites stated upfront. No implementation details unless directly necessary to execute the step.


### Reference

A document that defines a schema, API, data structure, or lookup table. Read selectively — the reader looks up a specific element, not the whole document.

**Examples:** JSON schema, API endpoint list, configuration key reference.

**Style:** Tabular and precise. Each entry is self-contained. No narrative prose. Units, types, and constraints always stated.


### Project file

Files that structure a project: `PROJECT.md`, `README.md`, `GLOSSARY.md`, `TODO/`. Each is governed by its own convention.

**Style:** Governed by the relevant convention — see `[[glossary-rules]]`, `conventions/todo-list.md`, `conventions/project-structure.md`.


### Log

Chronological records: session journals, decision logs, activity traces.

**Style:** Chronological, append-only. Entries dated. No imposed narrative structure.

## WWH by Document Type
[up](#table-of-contents)

| Type | Why | What | How |
|---|---|---|---|
| Process spec | Required | Required | Required |
| Convention | Recommended | Required | Required |
| Guide | Recommended | Optional | Required |
| Reference | Optional | Required | Required |
| Project file | — | — | — |
| Log | — | — | — |

**Required:** the level must be present and explicitly identifiable in the document structure.
**Recommended:** strongly encouraged — omitting it should be a conscious decision with a reason.
**Optional:** include when relevant to the document's purpose.
**—:** not applicable for this type.
