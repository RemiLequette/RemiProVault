# Glossary Rules

Convention for creating and maintaining a project's glossary.

*Document type: Convention — see [[documentation-style]]*

## Quick Start

Defines the format and rules for the `GLOSSARY.md` file present in every project.
Load when creating or modifying a glossary, or when auditing a project's documentation conformance.
Does not cover the business content of terms — only their form and organization.

## Load when
Creating or auditing a GLOSSARY.md

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

- Glossary Rules
    - Quick Start
    - Load when
    - Glossary Role
    - Location and Reference
        - Glossary File
        - Reference in PROJECT.md
    - GLOSSARY.md Structure
    - Domains
        - Definition
        - Domain Description
        - Suggested Domains
    - Terms
        - Format
        - Rules
    - Cross-References
    - Loading by an AI Assistant
    - Conformance and Audit
        - Conformance Criteria
        - Acceptable Deviations
```

## Glossary Role
[up](#table-of-contents)

The glossary defines the project's terms — business, technical, and local conventions.

It serves two types of readers:

**Human** — quickly understand what a term means without searching through the whole project.

**AI Assistant** — align its understanding with the project's terminology before starting any task, avoiding semantic misunderstandings.

## Location and Reference
[up](#table-of-contents)

### Glossary File

`GLOSSARY.md` is placed at the project root.

### Reference in PROJECT.md

Every `PROJECT.md` must contain a `## Glossary` section pointing to this file:

```markdown
## Glossary

See `GLOSSARY.md` at project root.
```

The absence of this section in `PROJECT.md` is a deviation to justify during audit.

## GLOSSARY.md Structure
[up](#table-of-contents)

```markdown
# Glossary — [Project Name]

## Quick Start

Glossary of key terms for the [Project Name] project.
Organized by domain. Read each domain description to assess
relevance before loading its content.

## Load when
Looking up project-specific terminology

## [Domain 1]

[Informative domain description — see Domains section]

### [Term A]
**Definition:** ...
**Example:** ... (optional)
**See also:** ... (optional)

### [Term B]
**Definition:** ...

## [Domain 2]

[Informative domain description]

### [Term C]
**Definition:** ...
```

## Domains
[up](#table-of-contents)

### Definition

A domain is a thematic grouping of terms. Each project defines its own domains freely, based on its nature.

### Domain Description

Each domain section opens with a 2-to-4-line informative description.

This description lets an AI Assistant quickly determine whether the domain is relevant to the current task, without loading all its terms.

```markdown
## UI

Terms related to the user interface: visual components, interactions,
display states, navigation. Relevant for any task touching screen
rendering or behavior.
```

### Suggested Domains

Non-exhaustive list — adapt to the project:

| Domain | Typical content |
|--------|-----------------|
| `UI` | Visual components, interactions, display states |
| `Data Model` | Entities, relationships, data structures |
| `Workflows` | Processes, steps, state transitions |
| `API` | Endpoints, parameters, response codes |
| `Configuration` | Variables, environments, system settings |
| `Business Rules` | Business rules, constraints, edge cases |
| `Roles` | Actors, permissions, responsibilities |
| `Infrastructure` | Servers, deployment, environments |

## Terms
[up](#table-of-contents)

### Format

```markdown
### [Term]
**Definition:** Clear, concise description of the term in the project's context.
**Example:** (optional) Concrete illustration.
**See also:** (optional) Pointer to related terms.
```

### Rules

- A term is defined in its **primary domain** only
- The definition is project-specific — not a generic definition
- No internal jargon left undefined within the definition itself

## Cross-References
[up](#table-of-contents)

A term may belong to several domains. It is defined once, in its primary domain. Other domains reference it:

```markdown
## Workflows

### Session
-> See [Data Model — Session](#session)
```

**Rule:** never duplicate a definition. One single source of truth per term.

## Loading by an AI Assistant
[up](#table-of-contents)

At session start, the AI Assistant:

1. Loads `GLOSSARY.md` in full
2. Reads each domain's description
3. Identifies the domains relevant to the current task
4. Loads the term content of those domains only

This strategy avoids loading irrelevant terms and optimizes use of the context window.

## Conformance and Audit
[up](#table-of-contents)

### Conformance Criteria

| Element | Conformant | Non-conformant |
|---------|------------|-----------------|
| `PROJECT.md` contains `## Glossary` | yes | deviation to justify |
| `GLOSSARY.md` exists at the root | yes | deviation to justify |
| Each domain has a description | yes | minor deviation |
| Each term has a definition | yes | minor deviation |
| No duplicated definitions | yes | minor deviation |
| Cross-references are consistent | yes | minor deviation |

### Acceptable Deviations

- Project too simple to need a glossary → document in `DEVIATIONS.md`
- Empty glossary at project start → acceptable, to be enriched over sessions
