# Project Ideas Inbox

Cross-project idea inbox — ideas posted from any project for any other project, pending triage.

*Document type: Log*

## Quick Start

Shared inbox for cross-project ideas.
Post an idea here when working in one project and thinking of something for another.
Triage is done on demand in the target project — read entries for your tag, promote to TODO or discard, then delete the line.
See `conventions/project-registry.md` for posting format and triage rules.

## Ideas
- `kb-maintenance/conventions` | Mettre à jour la description de `filesystem.md` dans `INDEX.md` — refléter l'ajout de la règle "large file generation via container + download" (keywords `download`, `present_files`, `large-files`). [effort: XS] [date: 2026-06-06]
- `kb-maintenance/conventions` | Nouvelle convention : "Artefact + outil de maintenance co-généré" — pattern à formaliser. Idée centrale : tout artefact volumineux à structure connue (HTML, JSON, Markdown long) devrait être accompagné dès sa création d'un outil de maintenance dédié (script Node.js ou Python) qui expose des opérations structurées sur ses sections : lire un bout, écrire un bout, lister des éléments. Claude invoque l'outil via `commands` MCP (zéro tokens), jamais le fichier brut. Analogie : c'est un "MCP local co-généré" — même philosophie qu'un vrai MCP (interface structurée, pas d'accès aux entrailles), mais spécifique à l'artefact, vivant dans le projet, sans serveur. Exemple existant dans guideIA : `guide-parser.js` + `plan-editor.html` jouent ce rôle pour `Plan.md` et `GuideIA.md`. Questions à trancher en session : nommage du pattern, structure minimale d'un outil de maintenance (interface CLI standard ?), quand le co-générer (toujours ? seulement au-delà d'un seuil ?), lien avec `conventions/tools.md` et `conventions/artifact.md`. [effort: M] [date: 2026-06-06]
- `kb-maintenance/conventions` | Convention méthodes de dev : tests (vitest) et logs (pino) — outillage standard pour tous les projets Node.js. Consolide les deux idées séparées (2026-06-08) en une seule convention : quand utiliser vitest, structure des tests, config globale ; quand logger, niveaux, format structuré JSON avec pino. [effort: S] [date: 2026-06-11]
- `kb-maintenance/conventions` | Droit à l'oubli — convention pour nettoyer les concepts périmés : critères d'obsolescence, workflow de suppression, pattern d'isolation anticipée (marquer un concept comme "candidat à la suppression" avant de le retirer). Motivé par la session FAL 2026-06-11. [effort: S] [date: 2026-06-11]
- `forge` | Handler .yaml/.yml — format natif Forge pour les fichiers YAML. Parser minimaliste key:value déjà en place dans md-extension-handler, à extraire et généraliser. [effort: S] [date: 2026-06-11]
- `kb-maintenance/conventions` | Bonnes pratiques pour une liste de gaps — même contenu que l'idée guide-ia. Extension : appliquer cette méthode dans les déviations d'audit — chaque déviation est traitée comme un gap, la liste est nettoyée (redondances, dépendances, ordre) avant de lancer les corrections. [effort: S] [date: 2026-06-10]
- `kb-maintenance` | Créer un viewer/explorateur de la registry des projets (`projects.md`) — outil HTML ou Node.js permettant de naviguer dans les projets enregistrés, leurs conventions, et leurs dépendances. [effort: M] [date: 2026-06-06]
- `kb-maintenance` | Bonne pratique : pas de référence en avant dans les conventions KB — chaque section ne doit utiliser que des concepts déjà introduits dans le document. [effort: XS] [date: 2026-06-10]
- `kb-maintenance` | Pattern mot-clé pour référencer un concept déjà défini — documenter le pattern dans best-practices.md ou glossary.md : un mot-clé stable évoque un concept sans le répéter, raccourcit les échanges, définit un contexte partagé. [effort: S] [date: 2026-06-10]

