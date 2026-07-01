---
id: T-002
Status: Done
importance: High
effort: L
type: todo
---

## Description

Remplacer le Decision Layer, la Scope Rule, et `INDEX.md` comme fichier de navigation par une recherche dynamique de contexte via le MCP Doc Index (une fois implémenté).

Principe : l'AI Assistant cherche au moment où le besoin apparaît (créer un `.md` → cherche "documentation conventions" ; bug SQL → cherche "sqlite write rules"), au lieu de charger une table trigger→fichier maintenue à la main.

La Scope Rule devient inutile car structurellement impossible à violer : le MCP Doc Index est scopé par repo (`repos.json`), pas de mécanisme pour sortir du repo courant.

`how-to-get-things-done.md` devient le premier réflexe de recherche en début de session (requête suggérée dans le bootstrap minimal de l'orchestrateur), plutôt qu'une lecture imposée.

## Notes

Dépend de l'implémentation du MCP Doc Index (`conventions/mcp-doc-index.md`) — le serveur existe et est en usage actif (`kb-doc-index`).

Question ouverte résolue : le bootstrap minimal de l'orchestrateur pointe désormais directement vers `PROJECT.md` (plus d'`INDEX.md`). `PROJECT.md` porte sa propre section `## Bootstrap Sequence` qui appelle `list_triggers(repo=kb)` au démarrage — pas une recherche à la demande pure comme envisagé initialement, mais le résultat pratique est le même : plus de table statique à charger, requête live sur `kb-doc-index`.

La Scope Rule a été abandonnée (session du 2026-07-01) suivant exactement l'argument ci-dessus.

`INDEX.md` a été supprimé (session du 2026-07-01). Toutes les références dans la KB (`project-structure.md`, `best-practices.md`, `README.md`, `PROJECT.md`, `GLOSSARY.md`, `tools.md`) ont été migrées vers le nouveau schéma.
