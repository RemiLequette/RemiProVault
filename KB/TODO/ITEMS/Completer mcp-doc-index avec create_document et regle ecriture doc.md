---
Status: Idea
importance: High
effort: M
type: todo
---

## Description

Compléter `conventions/mcp-doc-index.md` :

1. Ajouter un tool `create_document` au draft contract — `write_section` suppose actuellement un fichier déjà conforme ; sans un tool de scaffolding, impossible de créer un nouveau `.md` via le MCP seul (toujours besoin du filesystem pour la création initiale).
2. Une fois le serveur implémenté, ajouter une règle positive : les `.md` conformes à `documentation.md` sont lus et écrits exclusivement via les tools du Doc Index (`search`, `read_section`, `write_section`, `create_document`) — formulation positive, sans mentionner le filesystem (cf. anti-pattern "Signalling what was replaced").

## Notes

Le filesystem MCP reste nécessaire pour tout ce qui n'est pas `.md` conforme : code source, JSON, repos.json, fichiers non-documentation. "Retirer le filesystem" n'est vrai que pour le sous-ensemble doc, pas un remplacement total.

Pas de solution identifiée pour interdire structurellement (côté config MCP) l'accès filesystem aux `.md` — à creuser si on veut une contrainte architecturale plutôt que comportementale.

Ne pas rédiger la règle "positive" avant que le serveur soit réellement implémenté (actuellement in design).
