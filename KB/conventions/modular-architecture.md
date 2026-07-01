# Modular Architecture Convention

## Quick Start
Architecture convention for modular single-page applications.
Defines six concepts — Core, IService, Component, Page, Stylesheet, Assembly, Framework — and the project documents required to apply this convention.
Load when designing a new application following this convention, or when reasoning about where a concept, module, or service belongs.
Does not list concrete modules, service implementations, or named assemblies — those belong to the project docs referenced in each section.

## Load when
Modular architecture (Core, IService, Assembly, Framework)


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

- Modular Architecture Convention
    - Quick Start
    - Keywords
    - 1 - Overview
    - 2 - Core Modules
        - Fundamental UI Choice
        - Module Types
        - Module Order
    - 3 - Components
        - Component rules
        - Component registry
    - 4 - Page
        - Page registry
    - 5 - Stylesheet
        - Stylesheet registry
    - 6 - IService Interfaces
        - IService rules
        - Identifying an IService
    - 7 - Assemblies
    - 8 - Repository Layout
        - src/ — modules only
        - fragments/ — HTML page structure
        - styles/ — CSS
        - frameworks/ — deployment artifacts
    - 9 - Framework Convention
    - Index
    - Changelog
        - Version 6.0 - Page, Stylesheet concepts added — Assembly redefined
        - Version 5.0 - Modules only in src/ — fragments and styles promoted to top-level
        - Version 4.1 - Component exclusivity + src layout per component
        - Version 4.0 - Moved to KB as reusable convention
        - Version 3.7 - Component concept added
        - Version 3.6 - Framework provides IService implementations
        - Version 3.5 - IService and Framework concept corrections
        - Version 3.4 - Framework concept added to Overview
        - Version 3.3 - Framework Convention section + src/frameworks/
        - Version 3.0 - Réécriture convention-first
```

## 1 - Overview
An application following this convention is structured around four concepts.

**Core** — the framework-agnostic business logic of the application. Core is composed of Modules that depend only on abstract service interfaces (IService) — never on concrete implementations. Modules are grouped into Components, which describe the application's sub-domains and drive assembly composition. Core is the portable, ownable heart of the application.

**IService** — the interface pattern for any external capability required by Core. Each IService is an abstraction: Core programs against the interface; the concrete implementation is injected at assembly time. Multiple implementations of an IService can exist; one is selected per assembly.

**Component** — a named group of modules for assembly composition. See section 3.

**Page** — an ordered sequence of HTML fragments that defines the UI structure of one application. An assembly contains exactly one Page. See section 4.

**Stylesheet** — an ordered set of CSS files associated with a Page. An assembly contains exactly one Stylesheet. See section 5.

**Assembly** — a named, buildable application: one framework + one Page + one Stylesheet + one or more Components + one concrete IService implementation per IService consumed. One assembly = one deployable application. See section 7.

**Framework** — both a build-time and a runtime concept. At build time, it assembles the selected artifacts into an executable version. At runtime, it provides the host shell and may expose services through coupled IService implementations. A framework defines its own convention listing the IService implementations it provides and any artifacts it requires.

Each concept has a dedicated project document:
- Modules and Components → see the project Module Registry
- Pages and Stylesheets → see the project Views Registry
- IService interfaces and implementations → see the project IService docs
- Assemblies → see the project Assemblies doc

## 2 - Core Modules
Core is decomposed into modules. Each module has a type that determines how it is loaded and ordered at runtime.

### Fundamental UI Choice

Core is a single-page web application. Its UI is built on native HTML/CSS/JavaScript — no framework-specific toolkit, no virtual DOM, no component framework. This keeps Core portable: any host capable of rendering a web page can host it.

### Module Types

A **module** is a JavaScript unit with a declared responsibility, a public API, and testability as a first-class property. Only `script` blocks qualify as modules.

| Type | Content | Role |
|---|---|---|
| script | JavaScript only | Business logic, data, services — no markup. **Modules.** |

`style` and `div` blocks are **not modules** — they are presentation artifacts with distinct rules. See section 6 - Repository Layout.

### Module Order

Modules are ordered. The order is significant: it defines the sequence in which scripts are executed. Module order is defined in the project Module Registry.

The project Module Registry is the authoritative source for all modules: identity, type, layer, responsibility, dependencies, and testability. Naming convention: `{ProjectName}_Modules.md`.

## 3 - Components
A Component is a named group of modules. It is a logical grouping — not a deployment unit, not a package boundary. Its purpose is dual: architectural description (naming sub-domains of the application) and assembly composition (selecting a set of modules to include in a build).

Components are defined in the project Module Registry alongside the modules themselves.

### Component rules

- Every module belongs to exactly one Component — no module may be unassigned, no module may appear in more than one Component.
- Components do not overlap — the sets of modules they contain are disjoint.
- Components are named — the name reflects the sub-domain or capability they represent (e.g. core, ai, ui).
- An assembly selects one or more Components. The set of modules in the assembly is the union of all modules in the selected Components.
- An assembly must provide an IService implementation for every IService consumed by any module in its selected Components — no more, no less.

### Component registry

Components are defined in the project Module Registry (`{ProjectName}_Modules.md`), alongside the module entries. Each Component lists its member modules.

## 4 - Page
A Page is an ordered sequence of HTML fragments that reconstructs a complete UI. Fragments are concatenated in position order at build time — order is semantically significant; a misplaced fragment corrupts the DOM.

**One assembly = one Page.** A Page is not a route or a view inside an application — it is the entire HTML structure of one deployable application. Multiple applications may exist in a project, each with its own Page and assembly.

Fragments are not modules — they have no testable API and are not subject to Component rules. Their registry is the project Views Registry.

### Page registry

Pages and their fragments are defined in the project Views Registry (`{ProjectName}_Views.md`).

## 5 - Stylesheet
A Stylesheet is an ordered set of CSS files associated with a Page. CSS files are applied in position order — order matters for cascade and specificity.

**One assembly = one Stylesheet.** A Stylesheet is always paired with its Page in the same assembly.

Styles are not modules — they have no testable API and are not subject to Component rules. Their registry is the project Views Registry alongside Pages.

### Stylesheet registry

Stylesheets and their CSS files are defined in the project Views Registry (`{ProjectName}_Views.md`), alongside Pages.

## 6 - IService Interfaces

An IService is an abstraction boundary between Core and any external capability. Core calls the interface — it never depends on a concrete implementation.

### IService rules

- Core modules call the interface only — never a concrete implementation directly.
- Each IService has a dedicated project doc (`{ProjectName}_Service_{IServiceName}.md`) specifying its API and listing its implementations.
- A concrete implementation may depend on the host framework or external tools; Core never does.

### Identifying an IService

An external capability warrants an IService when:
- It could be replaced by a different implementation (e.g. different rendering library, different storage backend).
- Its concrete implementation would introduce a framework or platform dependency into Core.

The list of IService interfaces defined for the project is maintained in the project Assemblies doc.

## 7 - Assemblies
An assembly is a named, buildable application: one framework + one Page + one Stylesheet + one or more Components + one concrete implementation per IService consumed by those Components.

**One assembly = one deployable application.** Different assemblies in the same project produce different applications — each with its own Page, Stylesheet, and selected Components.

The project Assemblies doc (`{ProjectName}_Assemblies.md`) defines:
- The list of available frameworks and their block model.
- The list of concrete IService implementations and their framework constraints.
- The named assemblies: selected Page, Stylesheet, Components, framework, and IService implementation per consumed IService.
- The source synchronisation convention between the repository and the host framework.

## 8 - Repository Layout
The repository separates four concerns at the top level: modules, fragments, styles, and framework artifacts.

```
src/                         # JavaScript modules only — testable (Vitest)
  {component-name}/          # One folder per Component — matches component names exactly
    module-a.js
    module-b.js
  services/
    IServiceName/
      interface.js             # IService abstract definition
      implementation-1/        # A concrete implementation
      implementation-2/        # Another concrete implementation (optional)
fragments/                   # HTML fragments — one folder per Page
  {page-name}/               # Matches Page name exactly
    fragment-NNN-name.html   # NNN = position; order is semantically significant
styles/                      # CSS files — one folder per Stylesheet
  {stylesheet-name}/         # Matches Stylesheet name exactly
    style-NNN-name.css       # NNN = position; order is semantically significant
frameworks/                  # Framework deployment artifacts (sync trackers, config, scripts)
  {framework-name}/
```

### src/ — modules only

Only JavaScript module files live under `src/`. Each Component has its own folder; the folder name matches the Component name exactly. No module files at the `src/` root.

Each IService has its own folder under `src/services/`. The interface definition (`interface.js`) is always present. Concrete implementations are subfolders.

### fragments/ — HTML page structure

Fragments are ordered HTML files. They are concatenated in position order to produce the page. Order is semantically significant — a misplaced fragment corrupts the DOM. Fragment files are named with a position prefix (`fragment-NNN-name.html`) to make order explicit in the filesystem.

Fragments are not modules — they have no testable API. They are first-class artifacts of the presentation layer.

### styles/ — CSS

CSS files are ordered by position prefix (`style-NNN-name.css`). Order matters for cascade and specificity.

Styles are not modules — they have no testable API.

### frameworks/ — deployment artifacts

`frameworks/` is optional — it only appears if at least one framework requires artifacts. See section 7 for the framework convention.

## 9 - Framework Convention
A framework is the host shell that packages and serves the application. Its only requirement from this convention: it must allow the repository to be synchronised with it to produce an executable version.

How the framework achieves this — its block model, its build pipeline, its tooling — is defined by the framework's own convention, not by this document.

A framework may provide coupled IService implementations (e.g. an AI proxy, an auth service). Its convention lists which IServices it provides. The project Assemblies doc takes this into account when defining named assemblies.

If a framework requires artifacts — synchronisation tracker, configuration, scripts, or coupled IService implementations — it places them in `frameworks/{framework-name}/`. The framework convention defines what goes there and how it is organised. A framework that has no such need requires no entry under `frameworks/`.

Multiple framework folders may coexist in `frameworks/` — one per framework, each reflecting the state of the last assembly built against it.
