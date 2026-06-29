# Guide Parser

Spec du format des fichiers `Plan/` et `GuideIA/`, et de la librairie `tools/lib/guide-parser.js`.

*Document type: Process spec*

*Language: French — ce document cible le projet guideIA francophone.*

## Quick Start

Spec de référence pour tout code qui lit les fichiers `Plan/Plan-chNN.md` ou `GuideIA/GuideIA-chNN.md`.
Couvre le format exact des deux familles de fichiers (délimiteur, sections, syntaxe des blocs), et l'API de la librairie partagée `guide-parser.js`.
Charger avant toute modification du parsing dans `render_html.js` ou `guide-parser.js`.
Ne couvre pas le contenu éditorial du guide — voir `Plan-meta.md` et `Methode.md`.

## Keywords
guide-parser, Plan, GuideIA, format, parsing, librairie, figures, encadrés, mots-clés, balises, API

## Table of Contents

1. [Pourquoi une librairie partagée](#pourquoi-une-librairie-partagee)
2. [Format des fichiers Plan/](#format-des-fichiers-plan)
3. [Format des fichiers GuideIA/](#format-des-fichiers-guideia)
4. [API de guide-parser.js](#api-de-guide-parserjs)
5. [Index](#index)

## Pourquoi une librairie partagée
[up](#table-of-contents)

Les fichiers `Plan/` et `GuideIA/` sont parsés par `render_html.js`, qui assemble les chapitres et produit le rendu HTML. La librairie centralise le parsing — tout changement de format se fait en un seul endroit.

## Format des fichiers Plan/
[up](#table-of-contents)

### Structure générale

Chaque fichier `Plan/Plan-chNN.md` (ainsi que `Plan-introduction.md` et `Plan-conclusion.md`) contient deux zones séparées par un commentaire délimiteur HTML.

**Zone méta** — avant le délimiteur : objectif, sommaire, mots-clés du fichier. Cette zone n'est pas parsée par `guide-parser.js`.

**Zone contenu** — après le délimiteur : le chapitre avec ses sections. C'est la zone parsée.

Le délimiteur est une ligne de commentaire HTML contenant le mot `CONTENU` (en majuscules) :

```
<!-- ============================================================
     CONTENU DU GUIDE
     ============================================================ -->
```

Le parser identifie le délimiteur par l'expression régulière `/<!--[^\n]*CONTENU[^\n]*-->/i`.
Tout ce qui précède le délimiteur est ignoré.

### Chapitres

Chaque fichier de chapitre numéroté contient un titre de niveau 3 :

```
### N. Titre du chapitre
```

- `N` est le numéro du chapitre (entier, sans zéro de tête)
- Le séparateur entre numéro et titre peut être `.`, `-`, ou un espace
- Expression régulière : `/^### +(\d+)[.\-\s]+(.+)$/`

### Sections d'un chapitre

Chaque chapitre contient quatre sous-sections de niveau 4 :

| Section | Titre exact | Contenu |
|---------|-------------|---------|
| Mots-clés | `#### Mots-clés` | Liste de mots-clés |
| Figures | `#### Figures` | Blocs figure |
| Encadrés | `#### Encadrés` | Blocs encadré |
| Contenu | `#### Contenu` | Texte libre |

Une section s'étend de son titre jusqu'au prochain titre `####` ou `###` (ou la fin du chapitre).

Les sections peuvent être vides (titre présent, corps absent).

### Mots-clés

Format : liste Markdown, un mot-clé par ligne.

```markdown
#### Mots-clés
- stateless
- historique
- fenêtre de contexte
- token
```

Chaque ligne est lue par : `line.replace(/^-\s*/, '').trim()`.

### Figures

Format : un ou plusieurs blocs, séparés par des lignes vides.

```markdown
#### Figures

> 🖼️ **figure** `flux-orchestrateur`
> Diagramme du flux entre les deux composants :
> humain → prompt → orchestrateur → [...]
```

Chaque bloc commence par une ligne `> 🖼️ **figure** \`id\`` et contient optionnellement des lignes de légende (`> ...`).

Expression régulière pour extraire l'identifiant : `/>\s*🖼️\s*\*\*figure\*\*\s*`([^`]+)`/`

La légende est formée par la concaténation des lignes `>` suivantes (hors ligne d'identifiant), après suppression du préfixe `> `.

### Encadrés

Format identique aux figures, avec l'émoji 📦 et le mot `encadré` :

```markdown
#### Encadrés

> 📦 **encadré** `tokens`
> Définition des tokens : unité de mesure des LLM [...]
```

Expression régulière pour extraire l'identifiant : `/>\s*📦\s*\*\*encadré\*\*\s*`([^`]+)`/`

### Contenu

Texte libre en Markdown. Peut contenir des balises `{{def:...}}` et `{{ref:...}}` (voir section Format des fichiers GuideIA/). Peut être vide si le chapitre n'est pas encore rédigé dans le plan.

```markdown
#### Contenu

**Les deux composants**
L'architecture d'un assistant IA repose sur deux éléments distincts.
[...]
```

## Format des fichiers GuideIA/
[up](#table-of-contents)

### Structure générale

Chaque fichier `GuideIA/GuideIA-chNN.md` commence par un titre `## N. Titre` suivi du texte rédigé du chapitre. Il n'y a pas de délimiteur — le fichier entier est la zone parsée.

### Chapitres

Convention des niveaux de titres : voir `Methode.md`, section **Structure technique du guide** (source de vérité).

Le corps du chapitre est tout ce qui suit la ligne de titre, après suppression des espaces de tête et de queue.

### Balises

Le texte du guide contient des balises de définition et de référence pour les éléments nommés.

**Définition** — déclare l'occurrence principale d'un élément :

```
{{def:mk:hallucination}}
{{def:fig:flux-orchestrateur}}
{{def:enc:tokens}}
```

**Référence** — renvoie à une définition existante :

```
{{ref:mk:hallucination}}
{{ref:fig:flux-orchestrateur}}
```

Format : `{{(def|ref):(mk|fig|enc):identifiant}}`

- `mk` — mot-clé
- `fig` — figure
- `enc` — encadré

Ces balises sont résolues en HTML par `render_html.js`.

## API de guide-parser.js
[up](#table-of-contents)

### Contexte d'exécution

`guide-parser.js` est conçu pour fonctionner en Node.js, chargé via `require('./lib/guide-parser')` dans `render_html.js`. Le fichier utilise un wrapper UMD minimal pour être compatible aussi bien en Node.js qu'en navigateur si besoin.

### parsePlan(text)

Parse le contenu assemblé des fichiers `Plan/` et retourne la liste des chapitres.

**Paramètre :** `text` — contenu assemblé (string)

**Retour :** objet `{ chapters, parts }`.

```javascript
{
  chapters: [
    {
      num:     "3",                  // numéro du chapitre (string)
      title:   "Le modèle et l'orchestrateur",
      planContent: "**Les deux composants**\n...",  // texte de #### Contenu
      keywords: ["modèle de langage", "LLM", "orchestrateur"],
      figures: [
        {
          id:      "flux-orchestrateur",
          caption: "Diagramme du flux entre les deux composants : [...]"
        }
      ],
      encadres: [
        {
          id:    "tokens",
          title: "Définition des tokens : unité de mesure des LLM [...]"
        }
      ]
    },
    ...
  ],
  parts: [
    { num: "1", label: "Partie 1", pos: 0 },
    ...
  ]
}
```

- `num` : toujours une string (le numéro tel qu'il apparaît dans le fichier)
- `planContent` : texte brut de `#### Contenu`, vide si la section est absente
- `keywords` : tableau de strings, vide si `#### Mots-clés` est absent ou vide
- `figures` : tableau `{ id, caption }` — `caption` est la légende complète (lignes `>` concaténées)
- `encadres` : tableau `{ id, title }` — `title` est la première ligne de description

### parseGuide(text)

Parse le contenu assemblé des fichiers `GuideIA/` et retourne un dictionnaire indexé par numéro de chapitre.

**Paramètre :** `text` — contenu assemblé (string)

**Retour :** objet `{ [num]: chapitre }`.

```javascript
{
  "3": {
    content: "**Les deux composants**\nL'architecture [...]",
    defs: ["flux-orchestrateur", "prompt-systeme"],  // ids des {{def:...:id}}
    refs: ["hallucination"]                           // ids des {{ref:...:id}}
  },
  ...
}
```

- Clés : numéros de chapitres (strings), `'introduction'`, `'conclusion'`
- `content` : texte brut du chapitre (hors ligne de titre)
- `defs` : tableau des identifiants définis dans ce chapitre (tous types confondus)
- `refs` : tableau des identifiants référencés dans ce chapitre (tous types confondus)

### Fonctions internes

Les fonctions suivantes sont internes à la librairie et ne font pas partie de l'API publique :

- `parseFigBlocks(text)` — extrait les blocs figure d'une section `#### Figures`
- `parseEncBlocks(text)` — extrait les blocs encadré d'une section `#### Encadrés`
- `extractPlanContent(src)` — isole la zone contenu après le délimiteur

## Index

| Terme | Occurrences |
|-------|-------------|

## Changelog

### Version 1.2 - Adaptation structure par fichiers
**Date:** 2026-06-29
**Raison:** Le projet est passé de fichiers monolithiques `Plan.md` / `GuideIA.md` à une structure par chapitre (`Plan/Plan-chNN.md`, `GuideIA/GuideIA-chNN.md`). Mise à jour de la doc en conséquence. Suppression des références à `plan-editor` (outil retiré).

**Modifications :**
- Quick Start : retrait de la mention `plan-editor.html`
- Pourquoi une librairie partagée : section simplifiée, historique de la divergence retiré
- Format : renommage "Plan.md" → "fichiers Plan/" et "GuideIA.md" → "fichiers GuideIA/"
- API parsePlan : retour enrichi avec `parts`

---

### Version 1.1 - Renvoi vers Methode.md pour la convention des titres
**Date:** 2026-06-05
**Reason:** La section "Chapitres" de GuideIA.md dupliquait la convention des niveaux de titres au lieu de pointer vers la source de vérité (`Methode.md`). Suppression de la duplication, renvoi explicite.

**Changes:**
- Format de GuideIA.md / Chapitres : bloc de spec remplacé par un renvoi vers `Methode.md`

---

### Version 1.0 - Création
**Date:** 2026-06-05
**Reason:** Aucune source de vérité documentée pour le format de Plan.md et GuideIA.md. Ce document fixe le format de référence et spécifie l'API de la librairie partagée guide-parser.js.
