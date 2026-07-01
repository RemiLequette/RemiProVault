---
id: T-006
Status: Idea
importance: Low
effort: M
type: todo
---

## Description

Étudier la fusion de `conventions/todo-list.md`, `conventions/journal-changelog.md`, `conventions/glossary-rules.md`, `conventions/project-structure.md` en une convention commune "Project Scaffolding Files". Ces quatre conventions partagent le même squelette (fichier à la racine, format YAML/markdown fixe, règles AI Assistant identiques : proposer, attendre validation, écrire) et n'ont individuellement presque rien d'unique une fois ce format répété retiré.

## Notes

À ne considérer qu'après le principe de découplage taille fichier / accès IA (voir item séparé).

Lié au constat fait en session sur le glossaire : `conventions/glossary-rules.md` affirme un chargement automatique en session ("À l'ouverture d'une session, l'AI Assistant charge GLOSSARY.md en entier") qui n'a aucun point d'ancrage réel dans la Bootstrap Chain actuelle — à corriger indépendamment de la fusion, en basculant sur une interrogation à la demande via le MCP Doc Index plutôt qu'un chargement complet.
