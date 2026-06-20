# Plan Editor

Spec de l'outil de visualisation et d'édition du plan et du guide guideIA.

*Language: French — this document targets a French-speaking project.*

## Quick Start

Spec de l'outil `tools/plan-editor.html` — outil HTML standalone pour naviguer, éditer et vérifier la cohérence entre `Plan.md` et `GuideIA.md`.
Couvre le pourquoi de l'outil, le modèle conceptuel (vues, entités, cycle de vie des révisions), et l'architecture technique (stack, parsing, local server, save).
Charger avant toute modification de l'outil ou de son comportement.
Ne couvre pas le contenu éditorial du guide — voir `Plan.md` et `Methode.md`.

## Keywords
plan-editor, outil, HTML, local-server, révision, plan, guide, cohérence, balises, chapitres, éléments-nommés

## Table of Contents

1. [Why](#why)
2. [What](#what)
3. [How](#how)
4. [Index](#index)

## Why
[up](#table-of-contents)

Travailler sur `Plan.md` et `GuideIA.md` avec l'IA est efficace pour la valeur ajoutée — générer du contenu, reformuler, explorer des idées. C'est moins adapté pour naviguer dans la structure, comparer deux fichiers en parallèle, ou faire des modifications directes et précises sur le texte.

Un outil dédié prend en charge ce que l'IA fait mal :

**Navigation dans la structure** — visualiser les deux fichiers côte à côte par chapitre, sans scroller dans des fichiers longs. Voir d'un coup d'œil l'état d'un chapitre : ce qui est déclaré dans le plan, ce qui est rédigé dans le guide.

**Synchronisation plan / guide** — la synchronisation entre `Plan.md` et `GuideIA.md` est le cœur du travail éditorial : vérifier qu'un chapitre est bien développé dans le guide par rapport à ce qui était prévu dans le plan, ajuster l'un en regardant l'autre. Cette comparaison est impossible à faire mentalement sur des fichiers longs — elle nécessite une vue simultanée des deux. Les onglets forcent à basculer et à se souvenir de ce qu'on vient de lire. La vue côte à côte supprime ce coût cognitif.

**Cohérence structurelle** — les balises `{{def:...}}` et `{{ref:...}}` peuvent dériver sans qu'on s'en aperçoive. L'outil détecte en continu les incohérences entre les deux fichiers : balises manquantes, références invalides, chapitres désynchronisés.

**Gestion des éléments nommés** — ajouter, renommer, supprimer des mots-clés, figures et encadrés déclarés dans le plan.

**Édition du texte** — modifier le contenu d'un chapitre dans le plan ou dans le guide directement depuis l'interface, sans ouvrir l'éditeur.

**Modèle de révision explicite** — travailler directement sur les fichiers sources sans filet expose à des pertes en cas de fermeture accidentelle. Par ailleurs, un save immédiat à chaque modification ne donne aucun contrôle sur ce qui est écrit dans le Changelog. L'outil introduit un modèle de révision : les modifications s'accumulent dans une révision locale, clairement visibles, et ne sont appliquées aux fichiers sources qu'au moment d'un Save explicite.

## What
[up](#table-of-contents)

### Entités

**Chapitre** — unité de base, identifié par son numéro et titre. Existe dans le plan ET dans le guide, ou seulement l'un des deux (état désynchronisé).

**Élément nommé** — mots-clés, figures, encadrés. Déclaré dans le plan, défini dans le guide (`{{def:...}}`), référencé dans le guide (`{{ref:...}}`). Trois états possibles : déclaré seulement / défini / référencé.

**Section de texte** — contenu rédigé d'un chapitre : section `#### Contenu` dans le plan, corps du chapitre dans le guide.

**Révision en cours** — ensemble des modifications accumulées depuis le dernier Save. Persistée automatiquement dans `tmp/plan-editor-draft.json`. Rechargée au démarrage si elle existe. Chaque élément modifié est clairement marqué comme tel dans toutes les vues.

### Vues

**Vue Navigateur** — panneau permanent affichant la structure des chapitres dans leur ordre courant. Indique les incohérences : chapitre présent dans un seul fichier, éléments non définis. La sélection d'un chapitre pilote les deux éditeurs et la vue éléments.

Chaque chapitre affiche un badge à droite, avec tooltip au survol :

| Badge | Couleur | Signification |
|-------|---------|---------------|
| ✓ | vert | Chapitre synchronisé — tous les éléments déclarés dans le plan ont un `{{def:...}}` dans le guide |
| ⚠N | jaune | N éléments déclarés dans le plan sans `{{def:...}}` dans le guide — détail visible dans la Vue Éléments |
| plan | bleu | Chapitre présent uniquement dans Plan.md |
| guide | bleu | Chapitre présent uniquement dans GuideIA.md |
| ✎ | orange | Modifications en attente dans la révision en cours (s'ajoute aux autres) |

**Vue Plan du chapitre** — panneau supérieur gauche, permanent. Affiche et permet d'éditer le texte de la section `#### Contenu` du chapitre sélectionné dans `Plan.md`.

**Vue Guide du chapitre** — panneau supérieur droit, permanent. Affiche et permet d'éditer le texte du chapitre sélectionné dans `GuideIA.md`.

**Vue Éléments du chapitre** — panneau inférieur, permanent. Affiche les mots-clés, figures et encadrés déclarés dans le plan pour le chapitre sélectionné, répartis en trois colonnes. Pour chaque élément, un badge indique s'il est défini dans le guide (`{{def:...}}` présent — badge vert ✓) ou seulement déclaré dans le plan (badge jaune ○). Actions disponibles : ajouter ou supprimer un élément.

### Cycle de vie de la révision

- **Démarrage** — charge les fichiers sources, renumérotation automatique si nécessaire (voir section dédiée), sélection du premier chapitre, révision en cours rechargée si elle existe
- **Modification** — toute modification est ajoutée à la révision et persistée localement au fil de l'eau
- **Save** — la révision est appliquée aux fichiers sources, le Changelog est mis à jour (après validation par l'utilisateur), la révision locale est effacée. Pas de renumérotation au Save — elle est faite au chargement.
- **Rafraîchir** — recharge les fichiers sources depuis le disque. Si une révision est en cours, demande confirmation avant de l'abandonner. Déclenche la renumérotation automatique si nécessaire.

### Contraintes

**Accès concurrent non géré** — l'outil ne détecte pas les modifications externes sur `Plan.md` et `GuideIA.md` pendant une révision en cours. L'utilisateur s'engage à ne pas modifier ces fichiers (via l'éditeur ou l'IA) tant qu'une révision est ouverte. Cette contrainte pourra être levée dans une version ultérieure (détection par timestamp ou hash).

## How
[up](#table-of-contents)

### Stack

Fichier unique `tools/plan-editor.html` — HTML + CSS + JS inline, aucune dépendance locale externe.
Dépendances externes chargées via Google Fonts uniquement (DM Sans, DM Mono).

Accès aux fichiers via `local-server.js` — lecture et écriture de `Plan.md`, `GuideIA.md`, et du fichier de révision.

### Fichiers accédés

| Fichier | Accès | Rôle |
|---------|-------|------|
| `Plan.md` | lecture / écriture au chargement (si renumérotation) / écriture au Save | Source plan |
| `GuideIA.md` | lecture / écriture au chargement (si renumérotation) / écriture au Save | Source guide |
| `tmp/plan-editor-draft.json` | lecture / écriture au fil de l'eau | Révision en cours |

### Bootstrap

Fichier d'entrée `tools/plan-editor-bootstrap.html`, ouvert via `file://` :
1. Vérifie la disponibilité du local server via `GET /ping`
2. Si disponible : redirige vers `http://localhost:3000/<chemin absolu>/tools/plan-editor.html`
3. Si indisponible : affiche "Démarrez le local server avant d'ouvrir cet outil"

### Démarrage de l'outil

1. Charge `Plan.md` et `GuideIA.md` via `GET /file`
2. Parse les deux fichiers (chapitres, éléments nommés, balises)
3. Charge `tmp/plan-editor-draft.json` si il existe — applique la révision en cours à l'état en mémoire
4. Renumérotation automatique si nécessaire — écrit les fichiers sources sans confirmation
5. Sélectionne le premier chapitre, affiche l'indicateur de connexion et l'état de révision

### Parsing

- **Chapitres** : sections `### N. Titre` dans chaque fichier
- **Éléments nommés** : sous-sections `#### Mots-clés`, `#### Figures`, `#### Encadrés` dans `Plan.md`
- **Balises** : `{{def:type:id}}` et `{{ref:type:id}}` dans `GuideIA.md`
- **Incohérences** : chapitres manquants d'un côté, éléments déclarés sans définition
- Parsing délégué à `guide-parser.js` — voir `tools/guide-parser.md` pour le format de référence

### Renumérotation automatique

Au chargement et au rafraîchissement, l'outil vérifie que les numéros des chapitres forment une séquence continue (1, 2, 3…). Si ce n'est pas le cas, il renumérotation automatiquement `Plan.md` et `GuideIA.md` sans confirmation :
- Les titres `### N.` sont mis à jour dans les deux fichiers
- La liste des chapitres en en-tête de `Plan.md` est mise à jour
- Un badge "↺ renuméroté" apparaît brièvement dans la topbar
- Si les numéros sont déjà en séquence, aucune écriture n'a lieu

Cette opération est silencieuse et sans Changelog — elle est considérée comme de la maintenance structurelle, pas une révision éditoriale. Elle est déclenchée by design uniquement au chargement et au rafraîchissement, jamais en cours de session.

### Format de la révision

`tmp/plan-editor-draft.json` contient une liste d'opérations typées :

| Opération | Données |
|-----------|---------|
| `edit-plan-content` | `{ chapter, content }` |
| `edit-guide-content` | `{ chapter, content }` |
| `add-element` | `{ chapter, elType, id }` |
| `remove-element` | `{ chapter, elType, id }` |

### Save

1. Applique les éditions de contenu (`edit-plan-content`, `edit-guide-content`) aux deux fichiers en mémoire
2. Génère une entrée Changelog (date + résumé des opérations, dédupliqué) — présentée à l'utilisateur pour validation
3. Écrit `Plan.md` et `GuideIA.md` via `POST /file`
4. Supprime `tmp/plan-editor-draft.json` via `DELETE /file`

### Layout

- Trois zones : Navigateur (fixe, gauche) | Zone chapitre (flex, reste de l'écran)
- Zone chapitre — rangée haute : deux panneaux éditables côte à côte, Plan (gauche) et Guide (droite), hauteur égale
- Zone chapitre — rangée basse : panneau Éléments, hauteur fixe, scrollable
- Aucun onglet — les trois panneaux de détail sont toujours visibles simultanément
- Indicateur de connexion (vert/orange/rouge) et badges "révision en cours" / "↺ renuméroté" visibles dans la topbar

## Index

| Terme | Occurrences |
|-------|-------------|

## Changelog

### Version 3.0 - Simplification navigateur + renumérotation automatique au chargement
**Date:** 2026-06-06
**Raison:** Le navigateur concentrait trop de fonctionnalités interactives (ajout, suppression, réordonnancement D&D) mieux gérées par l'IA en session. Priorité donnée à la visualisation. La renumérotation, auparavant manuelle au Save, est déplacée au chargement/rafraîchissement — opération de maintenance structurelle silencieuse.

**Modifications :**
- Why : suppression du paragraphe "Modification directe de la structure" (fonctionnalité retirée)
- What / Vue Navigateur : suppression de la description D&D, séparateurs +, poubelle, bouton + Nouveau, bouton × sur chaque chapitre
- What / Cycle de vie : Save — retrait de la renumérotation ; Démarrage et Rafraîchir — ajout de la renumérotation automatique
- How / Stack : suppression de SortableJS (dépendance externe retirée)
- How / Format de la révision : suppression des ops `add-chapter`, `remove-chapter`, `reorder-chapters`
- How / Réordonnancement et renumérotation : section supprimée
- How / Save : étapes suppression et renumérotation retirées
- How / Renumérotation automatique : nouvelle section
- How / Démarrage : étape 4 ajoutée (renumérotation automatique)
- How / Layout : topbar — ajout du badge "↺ renuméroté"
- plan-editor.html : SortableJS retiré ; `renderNav()` — séparateurs et bouton × supprimés, cursor grab → pointer ; `initSplitter()`, `initListSortable()`, `initTrashSortable()`, `renderTrash()`, `restoreCh()`, `confirmDeleteCh()`, `promptAddCh()`, `makeInsertSep()` supprimées ; `autoRenumber()` + `showRenumberBadge()` ajoutées ; `load()` — appel `autoRenumber()` après `buildState()`, reconstruction de l'état si changement ; zone poubelle et splitter supprimés du HTML

---

### Version 2.0 - Réordonnancement D&D et insertion fluide
**Date:** 2026-06-05
**Raison:** W2 — trois fonctionnalités liées : ligne + au survol pour insérer à la position souhaitée, D&D pour réordonner, D&D depuis la poubelle pour restaurer à une position précise. Renumérotation automatique au Save.

**Modifications :**
- How / Stack : ajout de SortableJS 1.15.2 (cdnjs) comme dépendance externe
- How / Format de la révision : `add-chapter` — `chapter` est désormais un num temporaire ; `reorder-chapters` — une seule op conservée (dernière remplace les précédentes) ; suppression du champ `position` devenu inutile
- How / Save : étapes 2 (suppression) et 3 (renumérotation) ajoutées ; description mise à jour
- How / Réordonnancement et renumérotation : nouvelle section décrivant le calcul du mapping et son application
- What / Vue Navigateur : description du D&D, des séparateurs +, de la restauration depuis la poubelle
- What / Cycle de vie : Save enrichi — mention de la renumérotation
- plan-editor.html : SortableJS chargé en `<head>` ; `renderNav()` — séparateurs `ch-insert` entre chaque item ; `initListSortable()` + `initTrashSortable()` — SortableJS sur liste et poubelle ; `promptAddCh(insertIdx)` — titre seulement, position en paramètre, num temporaire ; `computeRenumber()` + `renumberPlanText()` + `renumberGuideText()` + `renumberPlanChapterList()` — renumérotation au Save ; `save()` — étapes suppression et renumérotation ajoutées

---

### Version 1.5 - Splitter poubelle, suppression directe, restauration au survol
**Date:** 2026-06-05
**Raison:** La zone poubelle était trop basse et de hauteur fixe. La suppression demandait une confirmation redondante. La restauration n'était pas cohérente avec le geste de suppression.

**Modifications :**
- plan-editor.html : splitter redimensionnable entre `#chapter-list` et `#nav-trash`, hauteur mémorisée en localStorage (`plan-editor:trash-height`)
- plan-editor.html : suppression de chapitre sans confirmation modale — direct dans la révision, titre mémorisé dans l'op
- plan-editor.html : restauration via bouton ↩ visible au survol (même mécanique que ×)
- What / Vue Navigateur : description mise à jour (zone poubelle toujours visible, splitter, restauration au survol)

---

### Version 1.4 - Rafraîchir et zone poubelle
**Date:** 2026-06-05
**Raison:** Le bouton Abandonner était trop destructif et peu accessible. Les chapitres supprimés étaient invisibles dans la révision. Deux améliorations complémentaires.

**Modifications :**
- plan-editor.html : bouton Abandonner remplacé par Rafraîchir (recharge silencieuse, confirmation si révision en cours)
- plan-editor.html : zone poubelle en bas du navigateur — chapitres supprimés restorables via ↩
- What / Cycle de vie : Abandon remplacé par Rafraîchir
- What / Vues / Vue Navigateur : ajout de la zone poubelle

---

### Version 1.3 - Tooltips badges et documentation Vue Éléments
**Date:** 2026-06-05
**Raison:** Les badges du navigateur n'étaient pas documentés et n'avaient pas de tooltip. La Vue Éléments n'était pas décrite avec précision.

**Modifications :**
- plan-editor.html : ajout de tooltips sur les quatre badges du navigateur
- What / Vues / Vue Navigateur : tableau des badges avec signification et couleur
- What / Vues / Vue Éléments : description du badge ✓/○ par élément, clarification des actions disponibles

---

### Version 1.2 - Parsing délégué à guide-parser.js
**Date:** 2026-06-05
**Raison:** Les parsers de `Plan.md` et `GuideIA.md` étaient dupliqués dans `plan-editor.html` et `render_html.js` et avaient silencieusement divergé. Extraction dans `tools/lib/guide-parser.js` — librairie partagée, wrapper UMD.

**Modifications :**
- How / Stack : dépendance ajoutée — `tools/lib/guide-parser.js` chargé via `<script>` avant le script principal
- How / Parsing : note ajoutée — parsing délégué à `guide-parser.js`, voir `tools/guide-parser.md` pour le format de référence
- Implémentation : fonctions `parsePlan()`, `parseGuide()`, `parseFigBlocks()`, `parseEncBlocks()` supprimées de `plan-editor.html` ; remplacées par `const { parsePlan, parseGuide } = GuideParser`

---

### Version 1.1 - Vue côte à côte Plan / Guide
**Date:** 2026-06-05
**Raison:** Les onglets empêchent la comparaison simultanée plan/guide, qui est le cœur du travail éditorial.

**Modifications :**
- Why : ajout du paragraphe **Synchronisation plan / guide** — coût cognitif des onglets, valeur de la vue côte à côte
- What / Vues : Vue Navigateur mise à jour (pilote deux éditeurs + éléments) ; Vue Plan et Vue Guide deviennent des panneaux permanents simultanés ; Vue Éléments passe en panneau inférieur permanent
- How / Layout : remplacement des trois onglets par un split vertical (rangée haute Plan+Guide, rangée basse Éléments)

---

### Version 1.0 - Création
**Date:** 2026-06-05
**Raison:** Spec initiale de l'outil plan-editor — rédigée en session guideIA après chargement des conventions documentation, documentation-style, tools et local-server.

**Contenu initial :**
- Why : limites du travail IA seul, valeur de l'outil, modèle de révision explicite
- What : entités (chapitre, élément nommé, section de texte, révision), vues (navigateur, plan, guide, éléments), cycle de vie, contraintes
- How : stack, fichiers, bootstrap, parsing, format révision, save, layout
