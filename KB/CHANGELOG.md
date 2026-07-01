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

## 2026-06-30 — racine KB — Ménage des fichiers obsolètes

**Reason:** Suppression manuelle par l'utilisateur de fichiers cruft à la racine de la KB, non couverts par une convention.

**Changes:**
- `ai-assistant-best-practices.md` supprimé (doublon de `guides/best-practices.md`)
- `instructions bootstrap.md` supprimé (non référencé par aucune convention)
- `instructions générales.md` supprimé (non référencé par aucune convention)
- `claaude-project-wilth-filesystem` supprimé (fichier orphelin, pas de format reconnu)

## 2026-06-30 — PROJECT.md, README.md, INDEX.md — Suppression du préfixe public/

**Reason:** Le dossier `public/` n'existe plus dans la KB (conventions/ et guides/ sont à la racine), mais ces trois fichiers référençaient encore l'ancienne structure. Le moment et l'auteur de la suppression du dossier `public/` restent non documentés — aucune trace dans Journal.md ou CHANGELOG.md ne l'enregistre.

**Changes:**
- `PROJECT.md` — section Structure, KB Maintenance, Decisions projet, Audit : tous les chemins `public/...` → racine
- `README.md` — Quick Start, Quick Navigation, Structure, Architecture, Key Files, For Humans & AI, Next Steps : tous les chemins `public/...` → racine
- `INDEX.md` — Session Bootstrap étapes 1-2 : `public/guides/...`, `public/conventions/...` → racine

## 2026-06-30 — conventions/documentation.md, conventions/tools.md, INDEX.md — Suppression des références à md-doc

**Reason:** `md-doc.js` (et sa convention d'usage `md-doc-usage.md`) était documenté en détail dans `tools.md` et référencé comme obligatoire dans `documentation.md`, mais n'a jamais été implémenté (absent de `KB-tools/`). Déclaré obsolète — toute référence retirée. La génération automatique de la TOC sera reconsidérée via une autre approche (à documenter séparément).

**Changes:**
- `documentation.md` — Document Structure (skeleton + table), TOC Rule, AI Assistant workflow, Tooling : références `md-doc`/`md-doc-usage.md` remplacées par une note "maintenance manuelle, pas d'outil pour l'instant"
- `tools.md` — Structure, Catalogue (table + sous-section commandes), Adding a Tool (Steps 2 et 5) : entrées et références `md-doc.js` supprimées
- `INDEX.md` — table conventions/ : ligne `md-doc-usage.md` supprimée

## 2026-06-30 — conventions/mcp-doc-index.md — Why et Quick Start reformulés

**Reason:** Le Quick Start et le Why mentionnaient explicitement l'ancien rôle (TOC/md-doc) que le MCP Doc Index remplace, à l'encontre du principe "ne pas mentionner ce qu'on veut éviter" (anti-pattern documenté le même jour dans `documentation-style.md`). Le Why ne mentionnait pas non plus le remplacement d'`INDEX.md` lui-même, objectif pourtant central. Reformulation pour ne décrire que l'état cible, et étendre explicitement le scope à tout projet souhaitant indexer sa propre doc.

**Changes:**
- `## Quick Start` — retrait de la mention "Replaces the navigation role md-doc/TOC..."
- `## Why` — réécrit : explique le coût de maintenance manuelle d'`INDEX.md`, et le besoin équivalent pour d'autres projets

## 2026-06-30 — conventions/documentation-style.md — Anti-pattern "Signalling what was replaced" ajouté

**Reason:** Un cas réel rencontré en session (la phrase "Replaces the navigation role..." dans `mcp-doc-index.md`) a révélé qu'un principe discuté précédemment ("ne pas signaler ce qu'on veut éviter", surnommé "principe Ross Perot" en session) n'était écrit nulle part dans la KB. Ajouté comme anti-pattern de la section Why-What-How Hierarchy pour qu'il soit chargé automatiquement à chaque création/édition de `.md` via le Decision Layer.

**Changes:**
- `## Why-What-How Hierarchy [section Anti-patterns]` — nouvel anti-pattern "Signalling what was replaced" avec exemple et justification

## 2026-06-30 — conventions/journal-changelog.md — Retrait de toute référence au changelog inline et à la migration

**Reason:** La migration des changelogs inline vers `CHANGELOG.md` centralisé est terminée. Le fichier mentionnait encore l'ancien mécanisme (Quick Start, Keywords, section Changelog/Why) et une section "Migration" listée dans la TOC mais absente du contenu (incohérence pré-existante). Conformément à l'anti-pattern "Signalling what was replaced" ajouté le même jour dans `documentation-style.md`.

**Changes:**
- `## Quick Start` — retrait de "replacing inline `## Changelog` sections" et de la clause de chargement pour migration
- `## Keywords` — retrait du mot-clé "migration"
- TOC — retrait de la section "Migration" (Why/Process) qui n'avait pas de contenu correspondant
- `## Changelog [section Why]` — reformulé sans référence à "inline"

## 2026-07-01 — conventions/documentation.md — Index et Changelog inline éliminés, TOC relâché

**Reason:** Décision session : seules Quick Start et Load when restent obligatoires. Index ne servait à rien ; Changelog est centralisé dans CHANGELOG.md depuis la migration ; TOC n'a plus de seuil de déclenchement.

**Changes:**
- Document Structure : table et diagramme mis à jour, Index/Changelog retirés
- Language > Standard names : Index/Changelog retirés
- TOC Rule : statut Optional au lieu de conditionnel sur le nombre de sections
- Citations : forme "Index" retirée des citation forms
- AI Assistant workflow, Tooling : références obsolètes nettoyées

## 2026-07-01 — conventions/project-structure.md — Bootstrap Chain et Bootstrap Files migrés vers PROJECT.md + kb-doc-index

**Reason:** INDEX.md supprimé de la KB ; le bootstrap pointe désormais directement sur PROJECT.md, le decision layer est une requête live (list_triggers).

**Changes:**
- Bootstrap Chain : diagramme mis à jour
- Bootstrap Files : template d'orchestrateur réécrit (kb-doc-index au lieu de filesystem+INDEX.md)

## 2026-07-01 — guides/best-practices.md — Principes 3 et 6 migrés vers kb-doc-index

**Reason:** Idem — INDEX.md supprimé, decision layer maintenant via list_triggers.

**Changes:**
- Principe 3 (Decision Layer) et Principe 6 (Top-Down, Linear) : exemples et diagrammes mis à jour

## 2026-07-01 — PROJECT.md — Load when ajouté, références INDEX.md retirées

**Reason:** Conformité au nouveau REQUIRED_SECTIONS ; INDEX.md supprimé.

**Changes:**
- Ajout ## Load when (fichier toujours chargé, hors decision layer)
- Structure et KB Maintenance : références INDEX.md → kb-doc-index

## 2026-07-01 — GLOSSARY.md — Load when ajouté, termes INDEX.md/Index/Changelog retirés ou mis à jour

**Reason:** Idem.

**Changes:**
- Ajout ## Load when
- Domaine Knowledge Base : terme INDEX.md supprimé, Workflow/PROJECT.md mis à jour
- Domaine Session : Session Startup, Selective Loading mis à jour
- Domaine Documentation : Keywords/Index/Changelog/TOC redéfinis pour refléter la nouvelle convention

## 2026-07-01 — README.md — Load when ajouté, références INDEX.md retirées

**Reason:** Idem.

**Changes:**
- Ajout ## Load when
- Quick Start, Quick Navigation, Structure, Key Files, For Humans & AI, Next Steps : INDEX.md → PROJECT.md / kb-doc-index

## 2026-07-01 — conventions/tools.md — Step 6 (Update INDEX.md) retiré

**Reason:** Idem.

**Changes:**
- Adding a Tool : Step 6 supprimé, Step 2/5 nettoyés des références "Index" manuel
