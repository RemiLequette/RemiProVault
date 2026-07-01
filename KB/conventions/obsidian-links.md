# Obsidian Links Convention

## Quick Start

Defines the wikilink syntax for inter-document references inside an Obsidian vault — alternative to the `see path/file.md [section X]` citation form from `conventions/documentation.md`.
Covers link syntax, section/heading anchors, and when to use this form instead of the standard citation form.
Load when creating or editing references between documents that live inside an Obsidian vault.

## Load when
Creating or editing links/references inside an Obsidian vault


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

- Obsidian Links Convention
    - Quick Start
    - Keywords
    - Scope
    - When To Use
    - Wikilink Syntax
        - Examples
    - Anchor Format
    - Relation To The Standard Citation Form
    - Index
```

## Table of Contents

1. [Load when](#load-when)
2. [Scope](#scope)
3. [When To Use](#when-to-use)
4. [Wikilink Syntax](#wikilink-syntax)
5. [Anchor Format](#anchor-format)
6. [Relation To The Standard Citation Form](#relation-to-the-standard-citation-form)

## Scope
[up](#table-of-contents)

Applies to any document that lives inside an Obsidian vault and links to another document in the **same** vault. Does not apply to citations targeting a document in a different vault, a document outside any vault, or to projects that do not use Obsidian — those use `conventions/documentation.md [section Citations]`.

**Two separate vaults exist today — do not assume they are one:**

| Vault | Members |
|-------|---------|
| Vault A | Knowledge Base, GuideIA |
| Vault B | DDScope |

A wikilink only resolves within the vault it is opened in. A document in the KB (Vault A) must never wikilink to a DDScope document (Vault B), and vice versa — use the standard path citation form for any cross-vault reference. See `projects.md` for the authoritative, up-to-date list of projects and which vault each belongs to.

## When To Use
[up](#table-of-contents)

Use a wikilink when both the source and the target document live inside the **same** Obsidian vault (see the table in [Scope](#scope)). This gives the human reader a clickable, navigable link in Obsidian (graph view, backlinks, hover preview) — benefits the plain-text citation form does not provide.

If the target document is in a different vault, outside any vault, or the document being written is not itself read through Obsidian, use the standard citation form instead. **Cross-vault wikilinks silently fail to resolve** — Obsidian shows them as broken links with no warning at write time, so this is easy to get wrong if vault membership isn't checked first.

## Wikilink Syntax
[up](#table-of-contents)

| Form | Syntax | Target |
|------|--------|--------|
| Document | `[[Note Name]]` | The entire document |
| Heading | `[[Note Name#Heading]]` | A specific heading |
| Heading with alias | `[[Note Name#Heading\|display text]]` | A specific heading, displayed under a custom label |
| Block | `[[Note Name^block-id]]` | A specific block — rarely needed, avoid unless the target has no usable heading |

**Note name:** use the filename without the `.md` extension. Obsidian resolves by unique filename across the vault — no folder path needed as long as the filename is unique vault-wide.

### Examples

```
See [[DDScope_Commands]] for the full call site inventory.
See [[DDScope_Commands#0 - Target architecture - DDS_CMD]] for the target pattern.
See [[DDScope_Commands#3.16 DDS_NOTES_UI — SCRIPT TBD (FEAT-002)]|the Notes call sites]] for detail.
```

## Anchor Format
[up](#table-of-contents)

Obsidian resolves heading anchors differently from VS Code (see `conventions/documentation.md [section TOC Rule]` for the VS Code rules):

- The heading text is used **as written** — no lowercasing, no space-to-hyphen conversion
- Punctuation and special characters in the heading are kept as-is
- The heading must match the target heading's text exactly, including numbering and punctuation

This means a heading written `## 3.16 DDS_NOTES_UI — SCRIPT TBD (FEAT-002)` is linked as `[[Note#3.16 DDS_NOTES_UI — SCRIPT TBD (FEAT-002)]]` — copy the heading text verbatim rather than deriving a slug.

**Consequence:** headings still follow `conventions/documentation.md [section Section Structure]` for uniqueness within a document (duplicates break Obsidian resolution the same way they break VS Code anchors), but the character restriction in that section is written for VS Code anchor generation — Obsidian itself tolerates more punctuation in headings. Keep following the restriction anyway for documents that may also be read or tooled outside Obsidian.

## Relation To The Standard Citation Form
[up](#table-of-contents)

The two forms are not interchangeable choices of style — they are selected by context:

| Context | Form |
|---------|------|
| Both documents in the same Obsidian vault | Wikilink — `[[Note#Heading]]` |
| Documents in different vaults (e.g. KB → DDScope, or DDScope → KB) | Standard citation — `see path/file.md [section Heading]` |
| Either document outside any vault, or vault status unknown | Standard citation — `see path/file.md [section Heading]` |

Do not mix both forms for the same reference. A maintenance pass may convert all existing intra-vault standard citations to wikilinks at once — this is not required to happen only incidentally while editing a reference for another reason.
