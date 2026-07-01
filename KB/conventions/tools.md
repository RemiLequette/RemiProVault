# Tools Convention

## Quick Start

Convention for script-based tools — applies both to shared KB tools (`tools/` in the knowledge base) and to project-specific tools (`tools/` in any project).
Load when creating a new tool anywhere, invoking an existing one, auditing tool conformance, or working on the shared library.
Does not cover project-specific business logic — only tool structure, interface, module design, invocation patterns, and the full development lifecycle (scope → spec → code → tests → doc → catalogue).

## Load when
Node.js automation script, cross-project tool


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

- Tools Convention
    - Quick Start
    - Keywords
    - Rationale
    - Structure
    - Shared Library
        - Current modules
        - Why a shared library
    - Module Design Rules
        - Single responsibility
        - Stable interface, hidden implementation
        - No side effects at require time
        - Pure functions where possible
        - Explicit errors
        - Explicit naming
        - JSDoc on every export
        - CommonJS modules
    - Standard Interface
    - Script Self-Documentation
        - WHY — the auto-regenerable script
        - Required header block
        - Separation of concerns — technical vs business
    - Invocation
    - Output
    - Tests
        - Structure
        - Framework
        - Test types
        - Mandatory workflow
        - Test file interface
        - Naming
        - Convention reference
        - Regression tests
    - Catalogue
    - Adding a Tool
        - Step 1 — Decide the scope
        - Step 2 — Write the spec first
        - Step 3 — Write the script
        - Step 4 — Write tests first
        - Step 5 — Update the catalogue
        - Step 6 — Update INDEX.md if needed
```

## Table of Contents

1. [Load when](#load-when)
2. [Rationale](#rationale)
3. [Structure](#structure)
4. [Shared Library](#shared-library)
5. [Module Design Rules](#module-design-rules)
6. [Standard Interface](#standard-interface)
7. [Script Self-Documentation](#script-self-documentation)
8. [Invocation](#invocation)
9. [Output](#output)
10. [Tests](#tests)
11. [Catalogue](#catalogue)
12. [Adding a Tool](#adding-a-tool)

## Rationale
[up](#table-of-contents)

Tools — scripts, HTML pages, or any deterministic artifact — exist for two reasons:

- **Token efficiency** — mechanical tasks consume AI context without requiring reasoning
- **Reliability** — a deterministic process does not hallucinate, drift, or vary between runs

This principle applies to any tool form, not only Node.js scripts.

**Use a tool when:**
- The task is mechanical (formatting, extraction, filtering, listing, indexing)
- The output is deterministic — no judgment required
- The task recurs

**Use AI when:**
- The task requires interpretation, synthesis, or judgment
- Occasional imperfection is acceptable

**Do not use AI when:**
- The task is one-off and simpler to do inline
- Reliability and repeatability matter more than flexibility

## Structure
[up](#table-of-contents)

```
tools/
  lib/
    md-parser.js     # parse and access sections of a Markdown document
    fs-scan.js       # directory scan, file reading, path utilities
  tests/
    fixtures/        # reference files, never modified
    sandbox/         # working copies for tests that write, gitignored
    md-parser.test.js
```

**Rules:**
- Tools are flat in `tools/` — no subdirectories for tools themselves
- Shared code lives in `tools/lib/` — never inline shared logic in a tool script
- No `package.json`, no npm dependencies — Node stdlib + `lib/` only
- Tool name: `kebab-case.js`, action-first (`index-`, `extract-`, `show-`, `diff-`, `audit-`)
- Library module name: `kebab-case.js`, domain-first (`md-parser`, `fs-scan`)

## Shared Library
[up](#table-of-contents)

`tools/lib/` contains reusable modules shared across tools.
Tools `require` them with a relative path: `const md = require('./lib/md-parser')`.

### Current modules

| Module | Responsibility |
|--------|---------------|
| `md-parser.js` | Parse a Markdown file into a structured doc object; access sections by name |
| `md-renderer.js` | Render a parsed doc object to HTML — TOC, section anchors, back-to-top links. Depends on `marked` (npm). |
| `fs-scan.js` | Scan directory trees for `.md` files; read files safely; resolve paths |
| `server-core.js` | Pure filesystem logic for the local server — safePath, readFile, writeFile, deleteFile, listDir, parseAllowedRoots, parsePort |

### Why a shared library

- Tools stay thin — they orchestrate, they do not parse
- Convention changes (e.g. new required section) are fixed in one place
- Tools are readable without understanding Markdown parsing internals

## Module Design Rules
[up](#table-of-contents)

These apply to every file in `tools/lib/`.

### Single responsibility
One module = one domain. `md-parser.js` parses Markdown. `fs-scan.js` handles the filesystem.
Never mix concerns in one module.

### Stable interface, hidden implementation
Consumers depend on exported function signatures, not on internals.
Rename or refactor internals freely — never change a function signature without updating all callers.

### No side effects at require time
A `require('./lib/md-parser')` must do nothing — no file reads, no console output, no global state.
All work happens inside function calls.

### Pure functions where possible
Same input → same output. No hidden state, no mutation of arguments.
When a function must have side effects (file write), name it explicitly: `writeReport()`, not `process()`.

### Explicit errors
Throw with a descriptive message. Never return `null` or `undefined` silently on error.
```js
// Good
if (!fs.existsSync(filePath)) throw new Error(`File not found: ${filePath}`);

// Bad
if (!fs.existsSync(filePath)) return null;
```

### Explicit naming
Function names describe what they return or do.
```js
// Good
getKeywords(doc)      // returns string[]
isConformant(doc)     // returns bool
getSection(doc, name) // returns string | null

// Bad
get(d, 'kw')
check(doc)
```

### JSDoc on every export
One line minimum. Parameters and return type when non-obvious.
```js
/** Returns the Keywords section as an array of trimmed strings, or [] if absent. */
function getKeywords(doc) { ... }
```

### CommonJS modules
Use `require` / `module.exports`. No ESM (`import`/`export`) — tools run directly with `node`, no bundler.
```js
module.exports = { parseFile, getSection, getKeywords, isConformant, getIssues };
```

## Standard Interface
[up](#table-of-contents)

Every tool follows this contract:

```
node tools/<script>.js <required_args...>
```

**Arguments:**
- Positional only — no flags, no named options

**stdout:** Machine-readable data only — no progress lines, no status messages.
First line is always a status line:
- `OK` — success; subsequent lines are data (empty if none)
- `ERROR:<code>:<message>` — failure; no data follows

Error codes:
- `MISSING_ARG` — required argument missing
- `FILE_NOT_FOUND` — file path does not exist
- `FILE_ALREADY_EXISTS` — file already exists (e.g. on create)
- `SECTION_NOT_FOUND` — named section absent from document
- `PROTECTED_SECTION` — named section is mandatory and cannot be deleted
- `NOT_CONFORMANT` — target file fails conformance check
- `WRITE_ERROR` — file could not be written

**stderr:** Not used — stdout/stderr cannot be separated by the invoker.

**Exit codes:**
- `0` — always, including applicative errors (status is in stdout)
- `1` — reserved for unhandled crashes only

**Script header (required):**
```js
/**
 * <script-name>.js
 *
 * <One-line description of what it does.>
 *
 * Args: <arg1> <arg2>
 */
```

## Script Self-Documentation
[up](#table-of-contents)

### WHY — the auto-regenerable script

A script is never a source of truth — it duplicates logic that is already specified in conventions, guides, and specifications. This is intentional and valuable: **a well-documented script is auto-regenerable**.

If the header comments reference exactly the documents used to design the script, and capture the rare information not yet present in those documents, then anyone (or an AI assistant) can reconstruct the script from scratch by reading only those sources. The script becomes a live proof of its own specifications.

Corollary: **when a comment contains information absent from the referenced documents, it signals a documentation debt** — those documents should be updated. The script points to its own gaps.

### Required header block

Every script must open with a header block that includes:

```js
/**
 * <script-name>.js
 *
 * <One-line description of what it does — purely technical, no business logic.>
 *
 * References (documents used to design this script):
 *   - <path/to/convention-or-spec.md> [section if relevant]
 *   - ...
 *
 * Not yet in references (document debt — update the refs to absorb these):
 *   - <any rule or constraint captured here because it is absent from the docs above>
 *
 * Args: <arg1> <arg2>
 */
```

**Rules:**
- `References` lists every convention, guide, or specification consulted — not the full path if obvious, but enough to find it
- `Not yet in references` is the debt register — information that lives only here, not in any doc. Every entry is a candidate for a doc update. If empty, write `none`.
- The one-line description is **purely technical** — it says what the script does mechanically, never why the business needs it

### Separation of concerns — technical vs business

> **A script does not know about business logic.**

A script that copies, transforms, or injects data has no opinion about the meaning of that data. It does not know what a "completed meeting" means, whether a status is valid, or what the publication rules are. Those decisions belong to the operator.

Concretely: **no conditional logic based on data content** (checking a status value, validating a business state, applying a publication rule) belongs in a script. If such logic exists, it is a violation of this principle and should be removed.

This principle must appear explicitly in the script header under `Not yet in references` until it is absorbed into the relevant specification.

## Invocation
[up](#table-of-contents)

Tools are invoked via the `commands` MCP with `node` whitelisted at `safe` level.

```
commands:execute_command
  command: node
  args: ["tools/<script>.js", "<arg1>", "<arg2>"]
```

**Rules:**
- Always pass absolute paths for file/directory arguments
- Always parse stdout: first line is `OK` or `ERROR:<code>:<message>`
- On `OK`: consume remaining lines as data
- On `ERROR`: read the code to decide whether to correct and retry, or abort
- Never assume success without reading stdout

## Output
[up](#table-of-contents)

- All output goes to stdout, after the `OK` status line
- No output files unless the tool's explicit purpose is to write a file (e.g. `create`, `update`)
- No hardcoded paths inside scripts — paths always passed as arguments
- Output format: plain text lines for lists, JSON for structured data

## Tests
[up](#table-of-contents)

### Structure

```
tools/
  tests/
    fixtures/        # reference files, never modified by tests
    sandbox/         # working copies for tests that write, gitignored
    <subject>.test.js
    ...
```

**fixtures/** — static reference files checked into the repo. Tests that only read point here directly.

**sandbox/** — working copies. Before any test that writes, copy the fixture to sandbox. The sandbox is gitignored (except `.gitignore` and `README.md`). Bug files (files that triggered a bug) are stored in `fixtures/` with a descriptive name.

### Framework

Node stdlib only — `assert` native module. No npm dependencies.

### Test types

- **A priori** — written from the spec, before the code. Tests fail first, code makes them pass.
- **Regression** — added on each bug fix, before touching the code. The test proves the bug, then proves the fix.

### Mandatory workflow

**New tool:**
1. Write tests from the spec
2. Run — all must fail
3. Write the code
4. Run — all must pass

**Bug fix:**
1. Drop the problematic file in `fixtures/` with a descriptive name
2. Write a regression test that fails
3. Fix the code
4. Run — regression test must pass, no other test must break

**Tool modification:**
1. Update or add tests to reflect the new spec
2. Run — changed tests must fail
3. Modify the code
4. Run — all must pass

**Principle: a test that has never failed proves nothing.**

### Test file interface

Same stdout protocol as tools:
- `PASS: <test name>` — test passed
- `FAIL: <test name> -- <reason>` — test failed
- Exit code `0` if all tests pass, `1` if at least one fails

```js
// Standard test runner pattern
const assert = require('assert');
let failed = 0;

function test(name, fn) {
  try {
    fn();
    console.log('PASS: ' + name);
  } catch (e) {
    console.log('FAIL: ' + name + ' -- ' + e.message);
    failed++;
  }
}

// ... tests ...

process.exit(failed > 0 ? 1 : 0);
```

### Naming

- Test file: `<subject>.test.js` — one file per lib module or tool
- Test name: describes the behaviour, not the implementation
  - Good: `getSection returns null when section is absent`
  - Bad: `test getSection null`

### Convention reference

Each test must carry a `@convention` comment on the line immediately before the `test(` call.

When the test enforces a rule from a convention file:
```js
// @convention conventions/documentation.md [section Quick Start Rule]
test('toMarkdown: throws when document has no title', () => {
```

When no convention applies (edge case, purely technical behaviour):
```js
// @convention none — edge case, no specific convention applies
test('parseText: empty string produces null title and no sections', () => {
```

The comment is machine-extractable — it enables auditing which tests cover which conventions.

### Regression tests

When a bug is fixed, add a test named after the bug:
```js
test('regression: setSection inserts before Index when Changelog is absent', () => { ... });
```

## Catalogue
[up](#table-of-contents)

| Script | Description | Args |
|--------|-------------|------|
| `md-to-html.js` | Convert a Markdown document to a standalone HTML file with neutral CSS and generated TOC | `<source.md> <output.html>` |
| `local-server.js` | Shared local HTTP server — serves static files and exposes `/file`, `/dir`, `/ping` API for all projects | `<root1> [<root2> ...] [--port <port>]` |

## Adding a Tool
[up](#table-of-contents)

### Step 1 — Decide the scope

Is this tool useful beyond the current project? → **KB tool** — place in `knowledgebase/tools/`.
Is this tool specific to one project? → **Project tool** — place in `<project>/tools/`.
When in doubt, start as a project tool. Promote to KB only when a second project needs it.

Both types follow the same rules below.

### Step 2 — Write the spec first

Before writing any code, document the tool:
- **Why** it exists — what problem it solves, what triggers its use
- **What** it does — inputs, outputs, behavior, edge cases
- **How** it works — data flow, error handling, dependencies

For a **KB tool**: add a spec section in this document or a dedicated `.md` in `tools/`.
For a **project tool**: add a spec section in the project's `Methode.md` or a dedicated `tools/<name>.md`.

Follow `conventions/documentation.md` and `conventions/documentation-style.md` (type: *process spec*) when writing a doc file.
If the doc has a `## Table of Contents`, keep it in sync (insert the `insta-toc` codeblock once if missing).

### Step 3 — Write the script

Follow `## Standard Interface` and `## Script Self-Documentation` above.
Use `lib/md-parser.js` and `lib/fs-scan.js` — do not re-implement parsing or file scanning.
The script header must reference the spec written in Step 2.

### Step 4 — Write tests first

Write a priori tests in `tools/tests/<script>.test.js` before running the code.
See `## Tests` for the mandatory TDD workflow.

### Step 5 — Update the catalogue

For a **KB tool**: add a row to `## Catalogue` in this document.
For a **project tool**: add an entry in the project's own tool spec or `Methode.md`.
New or modified `.md` files are picked up automatically by `kb-doc-index` (lazy reindex) — no manual navigation file to update.
