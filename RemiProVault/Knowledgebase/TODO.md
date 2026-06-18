# TODO

Backlog léger de la Knowledge Base — idées, améliorations, tâches.

## Quick Start

Backlog léger de la Knowledge Base.
Contient les tâches ouvertes et en cours, classées par priorité.
Les tâches terminées sont archivées dans `TODO-archive.md`.

## Keywords
todo, backlog, knowledge-base, tâches, idées, améliorations

## Table of Contents

1. [Critical](#critical)
2. [High priority](#high-priority)
3. [Normal](#normal)
4. [Low priority](#low-priority)
5. [WIP](#wip)
6. [Done](#done)
7. [Index](#index)

## High priority

- [ ] [O4] Bootstrap robustesse | Convention `documentation.md` + `md-doc-usage.md` absente du template AssistantIA.md — un AI démarrant sur un sujet métier crée des .md non conformes. Ajouter règle de chargement dans AssistantIA.md + déclencheur sur création/modification .md. Constaté sur ComiteRSE-AfrSCM 2026-06-01.
- [ ] [O5] Scope Rule — accès vs autorisation | AI confond accès technique MCP avec autorisation Scope Rule. Clarifier dans INDEX : distinguer accès technique et accès autorisé, préciser comportement attendu de l'AI Assistant.
- [ ] [O6] Convention self-reference in artifacts | An AI reading a project file directly (e.g. `TODO.md`, `GLOSSARY.md`) without going through `INDEX.md` has no signal to load the relevant convention. Explore a lightweight mechanism for files to reference their own governing convention — e.g. a standard comment header, a frontmatter field, or a convention pointer in the file's Quick Start. Constaté sur guideIA 2026-06-05.
- [ ] [O9] specs/ document type | Distinguer conventions opérationnelles (chargées par trigger) et specs descriptives (chargées explicitement). Créer `public/specs/`, migrer `forge.md`. Ajouter type Spec dans `documentation-style.md`. Ajouter section `specs/` dans INDEX.md. Litmus test : une spec n'a rien à auditer. [effort: M]
- [ ] [O11] guide-maintenance.md conformance | Non-compliant: French content, obsolete reference to Claude.md, informal structure. Bring into line with documentation.md before next audit. [effort: S]

## Normal

- [ ] [O12] Convention journal.md | Create a convention for Journal.md files — structure, entry format, when to create, what to capture (decisions, key moments, files modified). Update INDEX.md decision layer trigger and PROJECT.md structure. Based on Journal.md created in KB session 2026-06-05.
- [ ] [O13] tools.md Quick Start — extend scope to reference HTML viewers/editors and `guides/editor-tool.md`. Currently limited to scripts only.
- [ ] [O14] Shared library scope — KB vs project | `tools.md` defines the KB vs project rule for scripts but does not cover shared libraries (`lib/`). Clarify: a `lib/` module that is specific to one project belongs in `<project>/tools/lib/`, not in the KB. The KB `tools/lib/` is reserved for modules reusable across projects. Constaté sur guideIA 2026-06-05.
- [ ] [O17] md-doc Index support | Inline anchor tags currently point nowhere — implement renderer support so index references resolve correctly.
- [ ] [O18] md-doc Citations support | Implement cross-document citation resolution. Consider renaming Citations to References.
- [ ] [O19] md-doc Changelog support | Add/update Changelog entries via md-doc — new command or extension of update.
- [ ] [O22] Hierarchy of Concern in structured reasoning | Add HOC as a principle in conventions/claude-structured-reasoning.md — two dimensions: (1) gravity: not all errors are equal, grade remarks by actual impact; (2) order: general before detail, strategy before tactics. Never get lost in implementation details before validating the approach. Identified during how-to-get-things-done post-mortem 2026-06-06.
- [ ] [O24] md-doc handler | Type handler Forge pour fichiers Markdown structurés. Chaque section devient un bloc nommé. Remplace l'outil md-doc actuel et son protocole JSON. Gain token maximal — tous les fichiers KB. [effort: L]
- [ ] [O27] Extend maintenance rules beyond guides | guide-maintenance.md covers guides only. Conventions and tools also need maintenance rules. Consider convention-maintenance.md + tool-maintenance.md, or extend guide-maintenance.md to cover all three. [effort: S]
- [ ] [O28] Best practices → todo.md reference | Vérifier que guides/best-practices.md référence la convention todo.md.
- [ ] [O40] gitignore logs dans project-structure.md | Ajouter dans `conventions/project-structure.md` la règle : tout projet inclut un `.gitignore` avec `*.log`. Auditable en regardant le .gitignore — c'est une convention.
- [ ] [O48] forge_read batch query | forge_read(path, queries=[]) — plusieurs dot-notation queries en un appel. Réduit les allers-retours pour les lectures multi-sections. Distinct de O53 (multi-fichiers). [effort: S]
- [ ] [O49] forge_ls récursif avec tailles | forge_ls(fal, depth=N) — arborescence complète jusqu'à profondeur N. Chaque entrée inclut size (caractères ou lignes) pour orienter la lecture sans ouvrir les blocs. [effort: S]
- [ ] [O51] idea-inbox convention — simplifier et clarifier | Rendre le registre projet plus simple et clair pour utilisation directe par un AI Assistant. Mieux le référencer dans INDEX.md (trigger + description). [effort: S]
- [ ] [O55] Decision Layer séparé — ressources externes (SDK, libraries, frameworks) | Créer une section dédiée dans `INDEX.md` pour isoler le knowledge des dépendances externes utilisées par les outils KB. Principe : avant de modifier du code qui dépend d'un package, lire ce qui est installé, pas ce qu'on croit savoir. Exemple concret : SDK MCP `@modelcontextprotocol/sdk` v1.29.0 — `McpServer.tool()` exige un Zod schema (pas un JSON Schema brut) ; solution = `Server` low-level avec `setRequestHandler(ListToolsRequestSchema, ...)` + `setRequestHandler(CallToolRequestSchema, ...)` depuis `@modelcontextprotocol/sdk/server/index.js` et `@modelcontextprotocol/sdk/types.js`. Source de vérité : `node_modules/<sdk>/package.json` (version) + `.d.ts` (API). [effort: M]
- [ ] [O56] forge.md / MCP Specs — remplacer par pointeur vers forge-tools.json | La section MCP Specs duplique forge-tools.json (descriptions + inputSchemas). Décision session 2026-06-15 : forge-tools.json est la source de vérité, outputSchema ajouté dans le JSON. MCP Specs doit devenir un pointeur + uniquement les comportements implicites non exprimables en JSON Schema. [effort: S]
- [ ] [O57] Validation bascule Forge — test MCP en conditions réelles | Redémarrer Forge et exécuter le cycle complet create → read → write → delete sur fichiers dummy .md et .js dans le sandbox depuis Claude via MCP. Croiser avec filesystem pour vérifier sur disque. Faire après implémentation Sprint endpoint-split. [effort: S]
- [ ] [O58] forge_ls no-arg — spec/code mismatch | forge.md v2.1 documente `{ roots: [{ name, url }] }` mais le code retourne `{ allowedDirectories: string[] }`. Aligner spec ou code. Constaté en session 2026-06-18 lors de la rédaction de working-with-forge.md. [effort: XS]

## Low priority

- [ ] [O1] Premature drafting anti-pattern | Drafting final wording before the concept is stable anchors the conversation too early and forces rewrites. Sharper rule: discuss the idea first, draft only when the shape is clear. Refine "Validate before acting" in Phase 1 or add as a new anti-pattern in how-to-get-things-done.md.
- [ ] [O3] Goldilocks principle | Recurring pattern in the KB: too little loses value, too much loses quality, the right level is in between. Instances: session length, prompt granularity. Document as a named standalone principle — referenceable from how-to-get-things-done.md and elsewhere.
- [ ] [O15] Multilingual document support | `documentation.md` and `md-doc` assume English as the only language. Projects in another language (e.g. French) currently work around this with a language exception declaration but `md-doc` rejects non-English TOC headings. Define a proper multilingual support model. Dépend de md-doc handler. Constaté sur guideIA 2026-06-05.
- [ ] [O16] Add Design document type | Add Design document to taxonomy in documentation-style.md. Define style, WWH obligations, examples. Deferred 2026-06-03.
- [ ] [O20] Guides as frameworks not constraints | KB guides describe recommended practice, not mandatory procedure. The human decides. The AI suggests, never enforces. Document this principle — potentially in best-practices.md or as a guide preamble convention.
- [ ] [O21] AI best practices mirror human best practices | Develop the idea that effective collaboration practices with an AI resemble those with a common collaborator — clear intent, defined scope, explicit feedback, clean closure. Potentially a section in best-practices.md or a standalone guide.
- [ ] [O25] js-clean handler | Type handler pour fichiers JS maîtrisés. Shebang `// @forge-type: js-clean` en première ligne. Blocs par fonction `fn:<name>`, sections `// ---`. Dépend de md-doc handler. [effort: L]
- [ ] [O26] INDEX conventions/ table restructured by concept | The table is organized by file — restructure around concepts, making filenames secondary. Non-trivial refactor. [effort: M]
- [ ] [O29] Rename artifact scripts to English | ouvrir-revision.js → open-revision.js, cloturer-revision.js → close-revision.js. Update references in artifact.md.
- [ ] [O30] Convention github-pages.md | Document static site publication pattern independently of any specific project. artifact.md currently references a project-level file.
- [ ] [O31] Convention git.md + git tagging | Commit before large rewrites, commit message format, when to suggest a commit, Git operations via commands MCP. Inclure : format des tags, quand tagger, workflow recommandé. [effort: S]
- [ ] [O32] Externalize Changelogs? | Changelogs rarely needed in context. Explore separate changelog.md files or shared CHANGELOG.md per folder. Evaluate token savings vs navigation cost and tooling impact (md-doc, conformance checks). md-doc pourrait automatiser la migration.
- [ ] [O33] Write a WWH design session guide | Document the method for designing a tool or feature using WWH: how to structure Why/What/How, how to iterate, when to update conventions before coding. Based on the todo-tool design session 2026-06-04.
- [ ] [O41] Forge — registry viewer | Build a viewer artifact for the forge registry (roots, types, artifacts) — using forge to read itself via `forge_ls`. HTML tool or React artifact.
- [ ] [O42] Separate KB public/ to own repo | Move public/ to a standalone repository, independent from the KB Maintenance project. [effort: L]
- [ ] [O45] Forge editor — rename forge-browser.html | Rename `forge-browser.html` to `forge-editor.html` to reflect its expanded role. [effort: XS]
- [ ] [O46] Forge editor — add forge_create | Add artifact creation (forge_create) to the Forge editor tool. [effort: S]
- [ ] [O54] Generic configurable MCP server — document as convention or guide | Pattern identified in Forge M2: a `*-tools.json` declares tools + handler paths; `mcp-server.js` is a reusable runner independent of any tool domain. Extract from Forge and document for reuse across projects. [effort: M]

## WIP

- [ ] [WIP] [W5] Forge v2 — voir ROADMAP.md | O48 (forge_read batch query) + O49 (forge_ls récursif avec tailles) restants. [effort: M]


## Done

- [x] [D1] Complete how-to-get-things-done guide | Memory rule + HOC anti-pattern added.
- [x] [D2] Enrich todo-list convention with WIP concept | WIP elevated to first-class session concept.
- [x] [D3] Guide — working session conduct | Covered by how-to-get-things-done.md.
- [x] [D4] Anti-patterns in working sessions | Integrated in how-to-get-things-done.md.
- [x] [D5] Forge — gitignore logs KB | `*.log` added to KB .gitignore.
- [x] [D6] INDEX Decision Layer litmus test + Forge vocabulary | Done.
- [x] [D7] Bug forge_write bloc ignoré — plain-text.js | plain-text.js v1.2.
- [x] [D8] forge.js — top-level error handler | forge.js v2.2.
- [x] [D9] forge — folder CRUD | forge_mkdir, forge_rmdir, forge_mvdir, forge_rndir implémentés.
- [x] [D10] RTFM principle — spec + guide | forge.md v6.3 + working-with-forge.md v1.0 + INDEX.md v3.2.
- [x] [D11] Streisand Effect — glossaire + nettoyage docs | GLOSSARY.md v1.1-1.2 + working-with-forge.md v1.1 + INDEX.md v3.3.
- [x] [D12] Brand principle — spec + glossaire | forge.md v6.4 : Brand principle specced. GLOSSARY.md v1.3 : Brand, Constrain Don't Forbid, Fail Fast Fail Clear, TDD. working-with-forge.md v1.2 : Brand section + pattern filename-without-FAL.
- [x] [D13] forge_create + forge_write existence guard | plain-text.js v1.3 + forge.js v2.4 + forge.md v6.5.
- [x] [D14] forge.md v7.0 — clean architecture | ArtifactRef, UrlRef, IRootRegistry, Brand before RTFM, force removed. forge.md v7.0 + working-with-forge.md v1.3.
- [x] [D15] forge.js modularisation | forge.js split into src/logger.js, src/type-registry.js, src/root-registry.js, src/mcp-tools.js. forge.js reduced to entry point + re-exports. All test imports unchanged.
- [x] [D16] Tests consolidés + O8 forge_read header | Tests migrés dans knowledgebase/tests/forge/. forge-testable.js déprécié. parseFAL unifié. Gates Brand+RTFM dans TypeRegistry. discover filtre par extension. forge-run.js + CLI + REPL. O8 implémenté.
- [x] [D17] O23 forge_describe | forge_describe implémenté dans mcp-tools.js. TypeRegistry.describe() gère default + described flag. Testé.
- [x] [D18] structured-text.js v3.0 — grammaire blocs | matchName, blocks.separators (regex, repeat, récursion, template), listBlocks, readBlock/writeBlock nommés, createArtifact squelette. forge-types.json v0.8.0. O44 couvert par le mécanisme générique.
- [x] [D19] O53 forge_read multi-fichiers | `paths[]` implémenté dans forge-read.js. forge-tools.json mis à jour. Tests unit + integration (forge-read-multi.test.js). forge.md v1.7 + dispatch flow mis à jour.
- [x] [D20] W52 — forge_write describe + guard natif + test + GLOSSARY nettoyé | (1) working-with-forge.md supprimé (session précédente). (2) forge-tools.json : describe forge_write enrichi — native vs structuré, outputSchema ajouté sur tous les tools. (3) forge-write.js : guard natif — erreur explicite si payload sans `content` sur fichier natif. (4) forge-write-native.test.js : 9 tests. (5) GLOSSARY.md v1.4 : RTFM et Brand supprimés. Bug découvert en session : forge_write avec payload structuré sur fichier natif écrasait silencieusement — corrigé par le guard.
- [x] [D21] W7 — Sprint endpoint-split | 5 nouveaux tool-handlers (forge-get-format, forge-create-native/structured, forge-write-native/structured), FormatRegistry.describe() enrichi, forge-create.js + forge-write.js supprimés. Tests unitaires + intégration. Tous les tests passent.

## Index

| Term | Occurrences |
|------|-------------|

## Changelog

### Version 5.23 - O58 forge_ls no-arg spec/code mismatch
**Date:** 2026-06-18
**Reason:** Écart constaté en session lors de la rédaction de working-with-forge.md — forge.md v2.1 documente roots avec URL mais le code retourne allowedDirectories.

**Modifications:**
- Normal: O58 ajouté

---

### Version 5.22 - Clôture session 2026-06-18 — forge_ls roots avec URL
**Date:** 2026-06-18
**Reason:** Session tests Forge. W7 fermé en début de session. forge_ls sans argument enrichi : renvoie `{ name, url }` par root. Test + implémentation (forge-ls.js + root-registry.js). forge.md v2.1. W5 mis à jour : O48 + O49 restants.

**Modifications:**
- WIP: W5 description mise à jour — O48 + O49 restants

---

### Version 5.21 - W7 terminé — Sprint endpoint-split done
**Date:** 2026-06-18
**Reason:** Tous les tests passent. W7 → Done (D21). W6 reste en open.

**Modifications:**
- WIP: W7 retiré
- Done: D21 ajouté — Sprint endpoint-split complet

---

### Version 5.20 - W7 ajouté (Sprint endpoint-split) + W6 rétrogradé open
**Date:** 2026-06-18
**Reason:** Session dédiée à l'implémentation + tests des 5 nouveaux tool-handlers Forge (endpoint-split). W7 créé en WIP pointant vers ROADMAP Sprint endpoint-split. W6 (validation MCP réelle) rétrogradé en open — à faire après l'implémentation.

**Modifications:**
- WIP: W7 ajouté — Sprint endpoint-split (E.1–E.8)
- WIP: W6 retiré du WIP → déplacé en open (section Normal)

---

### Version 5.19 - W52 done + O56 ajouté + forge_ls enrichi
**Date:** 2026-06-15
**Reason:** Session W52 : toutes les sous-tâches terminées. forge_ls également enrichi en session (multi-ls, depth, fichier → retourne lui-même, sections supprimées). O56 ouvert pour nettoyage forge.md / MCP Specs.

**Modifications:**
- WIP: W52 retiré (terminé)
- Done: D20 ajouté — W52 complet
- Normal: O56 ajouté — forge.md / MCP Specs → pointeur vers forge-tools.json

---

### Version 5.18 - O52 reformulé + INDEX.md nettoyé
**Date:** 2026-06-14

---

### Version 5.17 - O55 Decision Layer ressources externes
**Date:** 2026-06-14

---

### Version 5.16 - Session validation bascule Forge MVP-1
**Date:** 2026-06-14

---

### Version 5.15 - M2 : 2.4 livré — MVP-1 atteint
**Date:** 2026-06-12

---

### Version 5.14 - M2 : 2.3 livré
**Date:** 2026-06-12

---

### Version 5.13 - M2 : 2.1 + 2.2 livrés
**Date:** 2026-06-12

---

### Version 5.12 - M1 done, M2 actif + O54
**Date:** 2026-06-12

---

### Version 5.11 - W5 pointe sur ROADMAP.md
**Date:** 2026-06-12

---

### Version 5.10 - O48 réécrit v2 + O43/O50/O37/O47 archivés
**Date:** 2026-06-12

---

### Version 5.9 - Roadmap Forge + items v1 superseded
**Date:** 2026-06-12

---

### Version 5.8 - O53 forge_read multi-fichiers
**Date:** 2026-06-12

---

### Version 5.7 - W5 mis à jour — forge.md v1.0
**Date:** 2026-06-11

---

### Version 5.6 - W5 forge-formats.json v1
**Date:** 2026-06-11

---

### Version 5.5 - W1, W3, W4 archived
**Date:** 2026-06-11

---

### Version 5.4 - O52 working-with-forge.md adapt to Forge v2
**Date:** 2026-06-11

---

### Version 5.3 - W4 forge.md How passe 2
**Date:** 2026-06-10

---

### Version 5.2 - O51 idea-inbox simplification
**Date:** 2026-06-10

---

### Version 5.1 - O50 writeBlock leaf-only rule
**Date:** 2026-06-07

---

### Version 5.0 - structured-text.js v3.0 done + nouveaux items
**Date:** 2026-06-07

---

### Version 1.0 - Création
**Date:** 2026-05-30
**Raison:** Backlog léger de la Knowledge Base.
