---
Status: Done
importance: High
effort: S
type: todo
---

## Description

Renommer `getKeywords()` en `getLoadWhen()` dans `md-parser.js` et mettre à jour `getIssues()` pour vérifier la présence de `## Load when` au lieu de `## Keywords`.

Impacte aussi tous les callers de `getKeywords()` dans KB-tools (indexer.js à créer, et tout script existant qui l'utilise).

## Notes

Décision prise en session lors de la migration ## Keywords → ## Load when dans tous les docs KB.
Fait le 2026-07-01 : `getIssues()` vérifie maintenant `## Load when` et `REQUIRED_SECTIONS = ['Quick Start', 'Load when']` (Index et Changelog retirés aussi, cf. `conventions/documentation.md`). Corrigé dans `md-parser.js` ET `md-parser.cjs` (la version chargée au runtime par le serveur MCP).
`getKeywords()` n'a pas été renommé en `getLoadWhen()` — aucun appelant actif ne l'utilise (`indexer.js` lit `## Load when` directement via `getSection`), donc laissé tel quel comme fonction legacy sans risque.
