# Documentation Convention

## Quick Start
Universal convention for all Markdown files (referred to as "documents" in the rest of this convention) — structure, headings, navigation, traceability.
Load when creating or modifying a document, or when auditing documentation conformance.
Does not cover the business content of documents — only their form and organization.
Some file types are exempt — see ## Scope.
See [Tooling](#tooling) for why these rules are strict. See [Citations](#citations) for inter-document reference format.

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

- Documentation Convention
    - Quick Start
        - Scope
        - Document type declaration
    - Load when
    - Document Structure
    - Section Structure
        - Headings
            - Examples
        - Length and Splitting
        - Subsections
    - Language
        - Standard names
        - Language exceptions
    - Numbering
        - Usage
        - Rule
        - No mandatory registry
    - Quick Start Rule
        - Rule
        - Writing guidance
        - Purpose
        - What Quick Start is not
        - Placement
        - Format
    - Load when Rule
        - Rule
        - Format
        - Criteria
    - TOC Rule
        - TOC format
    - Citations
        - Citation forms
        - Rule
        - Obsidian vaults
    - AI Assistant workflow
    - Tooling
```

### Scope
This convention applies to all Markdown files except those governed by a specific convention.

| Type           | Convention                         | Remarque                   |
| -------------- | ---------------------------------- | -------------------------- |
| `GLOSSARY.md`  | `[[glossary-rules]]`               | Structure glossaire        |
| `Journal.md`   | `conventions/journal-changelog.md` | Append-only session log    |
| `CHANGELOG.md` | `conventions/journal-changelog.md` | Centralized file changelog |

For exempt files, apply their specific convention instead.

### Document type declaration

Every document must declare its type. This declaration is part of the document header, governed by `conventions/documentation-style.md`.

See `conventions/documentation-style.md [section Document Taxonomy]` for the format and valid types.

## Load when
Creating or editing a .md file
Auditing documentation conformance

## Table of Contents

1. [Load when](#load-when)
2. [Document Structure](#document-structure)
3. [Section Structure](#section-structure)
4. [Language](#language)
5. [Numbering](#numbering)
6. [Quick Start Rule](#quick-start-rule)
7. [Load when Rule](#load-when-rule)
8. [TOC Rule](#toc-rule)
9. [Citations](#citations)
10. [AI Assistant workflow](#ai-assistant-workflow)
11. [Tooling](#tooling)

## Document Structure
[up](#table-of-contents)

Canonical structure of any document. Elements must appear in this exact order.

```
# Title

Subtitle (an optional short, plain-text description, no markup)

*Document type: ...*       <- see documentation-style.md

*Language: ...*            <- optional, see Language; blank line required between declarations

## Quick Start             <- mandatory, see Quick Start Rule
## Load when               <- mandatory, see Load when Rule
## Table of Contents       <- optional, see TOC Rule - insert an `insta-toc` codeblock, rendered live by Obsidian for human readers

## Section 1               <- content sections
## Section 2
...
```

| Element | Status | Rule |
|---------|--------|------|
| `# Title` | Mandatory | Unique, plain text |
| Subtitle | Optional | Short, plain-text, no markup - placed immediately under `# Title` |
| Document type declaration | Conditional | Required - see `conventions/documentation-style.md` |
| Language declaration | Optional | Required only when not English - see [Language](#language). **Each declaration on its own line, separated by a blank line** to prevent Markdown from merging consecutive italic lines into a single paragraph. |
| `## Quick Start` | Mandatory | See [Quick Start Rule](#quick-start-rule) |
| `## Load when` | Mandatory | See [Load when Rule](#load-when-rule) |
| `## Table of Contents` | Optional | Human-only navigation aid, rendered live by the Insta TOC Obsidian plugin - see [TOC Rule](#toc-rule). Add if useful; no section-count threshold. |
| Content sections | At least 1 | See [Section Structure](#section-structure) |

**File history:** this convention defines no inline `## Index` or `## Changelog` element. All file history is tracked centrally in `CHANGELOG.md` - see `conventions/journal-changelog.md`. A file may still carry a `## Keywords` section informally if useful for citation anchors, but it is not part of the canonical structure and never required.

**Convention:** In this document, the word "section" refers exclusively to content sections - those in the content zone, after `## Table of Contents` (or after `## Load when` if no TOC exists). `Quick Start`, `Load when`, and `Table of Contents` are referred to by their name only, never as "sections".

**The names of `Quick Start`, `Load when`, and `Table of Contents` are fixed in English regardless of document language** - see [Language](#language).

**Excluded from TOC count:** `## Quick Start`, `## Load when`, `## Table of Contents`.

**Content section zone:** Content sections must be placed after `## Table of Contents` (or after `## Load when` if no TOC exists). Inserting content sections outside this zone is not permitted.

## Section Structure
[up](#table-of-contents)

### Headings

Headings are the basis for navigation anchors. Any non-standard character breaks anchors.

**Rule:** Headings (`#`, `##`, `###`) must consist only of:
- Alphanumeric characters
- Accented characters (`é`, `è`, `à`, `û`, etc.)
- Spaces
- Hyphens `-`

**Forbidden in headings:**
- Emojis
- Special punctuation (`.`, `:`, `!`, `?`, `—`, `'`, etc.)
- Symbols (`↑`, `⏸`, `✅`, etc.)
- Encoded characters (`%XX`)

**Uniqueness:** Each `##` heading must be unique within the document — duplicates break anchors.

#### Examples

| Incorrect | Correct |
|-----------|---------|
| `## 1. Overview ⏸️` | `## 1 - Overview` |
| `## Step 1: Load context` | `## Step 1 - Load context` |
| `## ✅ Results` | `## Results` |
| `## Layout — HTML↔scripts contract` | `## Layout - HTML and scripts contract` |


### Length and Splitting

A `##` section must cover a single topic, readable in one sitting.

If a section requires several distinct sub-topics, split it into separate sections. A section that is too long usually covers multiple subjects.

This rule also applies to readability for an AI Assistant: an overly large section increases loading cost without improving relevance.


### Subsections

Use `###` to structure internal content of a section without bloating the TOC — subsections do not appear in it.

If a `###` becomes an autonomous citation or navigation target, promote it to `##`.

## Language
[up](#table-of-contents)

The default language of all documents is **English**.

### Standard names

The following names are **fixed in English regardless of the document language**. Tools rely on these exact names to parse and process documents.

| Element | Fixed name |
|---------|------------|
| Quick Start | `## Quick Start` |
| Load when | `## Load when` |
| Table of Contents | `## Table of Contents` |

Content inside each of these may be written in any language. The name itself must never be translated.

### Language exceptions

**Tooling-imposed names:** When external tools or systems impose predefined names in another language (heading titles, field names, configuration keys), those names may be kept as-is. Declare the exception under the document title:

```markdown
# Document Title

*Language: English. Exception: [name] uses French — imposed by [tool/system name].*
```

**Document in another language:** If a document is intentionally written in another language, declare it under the title with a short justification:

```markdown
# Document Title

*Language: French — this document targets a French-speaking team.*
```

## Numbering
[up](#table-of-contents)

### Usage

Numbers assigned to items (BP#1, Rule 3, etc.) serve two purposes:
- **Internal navigation** — orientation within a long document
- **Discussion** — quick reference during a conversation with a human

They are contextual tools, not stable identifiers.

### Rule

Never cite a numbered item from another document. To reference an external idea, cite the document or a section by its title.

**Correct ✅**
```
See guides/best-practices.md
See guides/best-practices.md — Instruction Minimalism
```

**Incorrect ❌**
```
Implements BP#1, BP#2, BP#8
See Rule 3
```

### No mandatory registry

Since numbers are not inter-document identifiers, no registry is required.

## Quick Start Rule
[up](#table-of-contents)

### Rule

Every document must begin with `## Quick Start` describing it in 3 to 6 lines.

### Writing guidance

`## Quick Start` is the primary entry point for both humans and tools. It must be descriptive enough to allow relevance assessment without loading the full document.

- A search engine or AI tool uses it to decide whether to load this document at all
- A human uses it to decide whether to read further
- Keep it factual and specific — vague Quick Starts force full document loads

### Purpose

`## Quick Start` is an **orientation summary** — it answers the question "does this document concern me?"

Two types of readers have different needs:

**Human** — wants to quickly understand the theme and scope before deciding to read in detail. They scan, they do not read linearly.

**AI Assistant** — must decide at session start whether this document is relevant to the current task and worth loading into context. Each document loaded has a double cost: in **tokens** (limited context window) and in **relevance** (a context cluttered with irrelevant documents degrades response quality).

**Note:** When referring to an AI assistant in a document, do not use a specific name (Claude, Gemini, etc.) — use "AI Assistant".

`## Quick Start` must therefore answer:
- **Theme** — what this document is about
- **Scope** — what it covers and what it does not cover
- **Conditions** — in what situations it is useful to read or load it

Combined with `## Load when`, it allows a human or an AI to decide in seconds whether the document is relevant.

### What Quick Start is not

- Not an exhaustive summary of the content
- Not a list of headings (that is the role of the TOC)
- Not a step-by-step action guide
- Not a numbered enumeration of content ("X techniques", "Y principles") — these numbers diverge from actual content and provide no useful information to the reader

### Placement

Immediately after the `#` title and short description, before `## Load when`.

### Format

```markdown
## Quick Start

[Theme: what this document is about]
[Scope: what it covers / does not cover]
[Conditions: when to load or read it]
```

## Load when Rule
[up](#table-of-contents)

### Rule

Every document must have `## Load when` placed after `## Quick Start`, before `## Table of Contents`.

### Format

```markdown
## Load when
Trigger phrase 1
Trigger phrase 2
```

### Criteria

- One trigger phrase per line
- Each phrase describes a situation or task that warrants loading this document
- Written as a situation, not a keyword list ("Creating or editing a .md file", not "markdown, editing")
- Specific enough to avoid loading the document unnecessarily

## TOC Rule
[up](#table-of-contents)

**Status:** Optional. Add a `## Table of Contents` when useful for human navigation — no longer required based on section count.

**Production:** When present, the TOC is rendered live by the **Insta TOC** Obsidian plugin (`iLiftALot/insta-toc`) from an `insta-toc` codeblock - see [Format](#toc-format) below. It exists for human navigation inside Obsidian. AI Assistants do not rely on it: they navigate via the document index exposed by the MCP server - see `conventions/mcp-doc-index.md`.

**Limitation:** The codeblock's content is rendered only by Obsidian - readers outside Obsidian (GitHub, VS Code, raw file reads, an AI Assistant reading the file directly) see the bare codeblock, not a list of links. This is an accepted trade-off: TOC is now an Obsidian-only convenience.

### TOC format

```markdown
## Table of Contents

​```insta-toc
​```
```

The plugin populates the rendered view automatically from the document's headings — nothing else is written by hand.

## Citations
[up](#table-of-contents)

Inter-document citations must point to an identifiable, stable target. Numbers (`BP#1`, `Rule 3`) are not valid citation targets — they are contextual navigation tools, not stable identifiers.

### Citation forms

Use paths relative to the project root. Three forms, combinable:

```
see conventions/filesystem.md
see conventions/filesystem.md [section Optimal strategy by operation type]
see conventions/filesystem.md [keyword node]
```

| Form | Syntax | Target |
|------|--------|--------|
| Document | `see path/file.md` | The entire document |
| Heading | `see path/file.md [section Heading Title]` | A `##` heading |
| Keyword | `see path/file.md [keyword term]` | A term in an informal `## Keywords` section, if present |

### Rule

Never cite by number. To reference an idea from another document, cite the document or heading by its title.

**Correct ✅**
```
see guides/best-practices.md
see guides/best-practices.md [section Instruction Minimalism]
```

**Incorrect ❌**
```
Implements BP#1, BP#2
See Rule 3
```

### Obsidian vaults

When both the citing and the cited document live inside the **same** Obsidian vault, use the wikilink form instead of the path form above — see `conventions/obsidian-links.md`. The path form remains mandatory across different vaults (e.g. between the KB and DDScope) and whenever vault status is unknown.

## AI Assistant workflow
[up](#table-of-contents)

When creating or modifying `.md` files, always write directly to the target path via the MCP doc index — never display Markdown content in the chat. Markdown rendered inside the chat interface collides with the interface's own rendering, making the output hard to read and wasting tokens. The chat should contain only discussion and decisions.

| Situation | Approach |
|-----------|----------|
| Initial creation | `write_section` via MCP doc index |
| Section edit | `write_section` via MCP doc index — one section at a time |
| Large rewrite | Suggest a Git commit first, then rewrite section by section |
| Convert to HTML or PDF | Use `tools/md-to-html.js` via `commands` MCP: `node tools/md-to-html.js <source.md> <output.html>`. Ask the user for the output path if not specified. |

**If a `## Table of Contents` section is present, ensure it contains the `insta-toc` codeblock (insert once if missing). A Table of Contents is optional — do not add one solely to satisfy this rule.**

AI Assistants do not need to read or maintain the TOC for navigation — use the MCP document index instead (see `conventions/mcp-doc-index.md`).

This applies to both KB files and project files.

## Tooling
[up](#table-of-contents)

The strict rules in this convention exist to support tooling built on top of the documentation structure. Three categories of tools depend on it:

**Human navigation** — the `## Table of Contents` heading, when present, rendered live by the Insta TOC Obsidian plugin from an `insta-toc` codeblock. See [TOC Rule](#toc-rule).

**AI Assistant navigation** — not via TOC. AI Assistants query the document index exposed by the MCP server (search, section-level read) instead of loading or parsing a TOC. See `conventions/mcp-doc-index.md`.

**Viewers** — render documents as structured, navigable interfaces. They rely on heading names, order, and heading format to build navigation trees, breadcrumbs, and renderers. Any deviation breaks the rendering.

**Automation** — mechanical tasks that should not consume AI tokens: verifying conformance (missing required headings, empty `## Load when`, duplicate headings), generating skeletons for new documents. These tools are more reliable and cheaper than asking an AI to do the same work.

All three depend on the same invariants: fixed heading names, canonical order, unique headings, and standard anchor format. Tools identify headings by their exact name — which is why the names of `Quick Start`, `Load when`, and `Table of Contents` must never be translated or altered.
