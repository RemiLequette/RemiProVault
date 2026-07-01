---
id: T-009
Status: Idea
importance: Medium
effort: S
type: todo
---

## Description

Étudier l'ajout d'un tool `read_sections` (pluriel) au MCP `kb-doc-index`, permettant de lire plusieurs sections (potentiellement dans plusieurs fichiers) en un seul appel, plutôt qu'un `read_section` par section.

## Notes

Gain principal attendu : réduction du nombre de roundtrips (latence de session), pas économie de tokens significative — le contenu retourné est le même, seul l'overhead d'enveloppe par appel est mutualisé.

À ne considérer que si un pattern répété de plusieurs `read_section` consécutifs sur le même besoin apparaît en session. Pas observé à ce jour : les lectures groupées actuelles passent par `filesystem:read_multiple_files` sur des fichiers entiers (bootstrap), pas par des lectures multiples de sections ciblées.

Discuté en session du 2026-07-01.
