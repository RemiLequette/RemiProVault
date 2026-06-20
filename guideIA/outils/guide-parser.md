# Guide Parser

Spec du format de `Plan.md` et `GuideIA.md`, et de la librairie `tools/lib/guide-parser.js`.

*Document type: Process spec*

*Language: French — ce document cible le projet guideIA francophone.*

## Quick Start

Spec de référence pour tout code qui lit `Plan.md` ou `GuideIA.md`.
Couvre le format exact des deux fichiers (délimiteur, sections, syntaxe des blocs), et l'API de la librairie partagée `guide-parser.js`.
Charger avant toute modification du parsing dans `render_html.js`, `plan-editor.html`, ou `guide-parser.js`.
Ne couvre pas le contenu éditorial du guide — voir `Plan.md` et `Methode.md`.
Ne couvre pas l'outil plan-editor — voir `tools/plan-editor.md`.

## Keywords
guide-parser, Plan.md, GuideIA.md, format, parsing, librairie, figures, encadrés, mots-clés, balises, API

## Table of Contents

1. [Pourquoi une librairie partagée](#pourquoi-une-librairie-partagee)
2. [Format de Plan.md](#format-de-planmd)
3. [Format de GuideIA.md](#format-de-guideiamnd)
4. [API de guide-parser.js](#api-de-guide-parserjs)
5. [Index](#index)

## Pourquoi une librairie partagée
[up](#table-of-contents)

`Plan.md` et `GuideIA.md` sont parsés par deux consommateurs distincts :

- `render_html.js` — script Node.js qui génère `output/GuideIA.html`
- `plan-editor.html` — outil HTML qui lit et écrit les deux fichiers via le serveur local

Sans librairie partagée, chaque consommateur maintient son propre parser. Quand le format évolue, il faut mettre à jour les deux — et le risque de divergence est élevé. C'est exactement ce qui s'est produit : au moment de la création de `guide-parser.js`, les deux parsers avaient déjà divergé sur la syntaxe des blocs figures et encadrés.

La librairie centralise le parsing. Tout changement de format se fait en un seul endroit.

## Format de Plan.md
[up](#table-of-contents)

### Structure générale

`Plan.md` contient deux zones séparées par un commentaire délimiteur HTML.

**Zone méta** — avant le délimiteur : objectif, sommaire, guide de style figures, liste des chapitres. Cette zone n'est pas parsée par `guide-parser.js`.

**Zone contenu** — après le délimiteur : chapitres avec leurs sections. C'est la zone parsée.

Le délimiteur est une ligne de commentaire HTML contenant le mot `CONTENU` (en majuscules) :

```
<!-- ============================================================
     CONTENU DU GUIDE
     ...
     ============================================================ -->
```

Le parser identifie le délimiteur par l'expression régulière `/<!--[^\n]*CONTENU[^\n]*-->/i`.
Tout ce qui précède le délimiteur est ignoré.

### Chapitres

Chaque chapitre est introduit par un titre de niveau 3 :

```
### N. Titre du chapitre
```

- `N` est le numéro du chapitre (entier, sans zéro de tête)
- Le séparateur entre numéro et titre peut être `.`, `-`, ou un espace
- Expression régulière : `/^### +(\d+)[.\-\s]+(.+)$/`

Un chapitre s'étend du titre jusqu'au début du chapitre suivant (ou la fin du fichier).

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

Texte libre en Markdown. Peut contenir des balises `{{def:...}}` et `{{ref:...}}` (voir section Format de GuideIA.md). Peut être vide si le chapitre n'est pas encore rédigé dans le plan.

```markdown
#### Contenu

**Les deux composants**
L'architecture d'un assistant IA repose sur deux éléments distincts.
[...]
```

## Format de GuideIA.md
[up](#table-of-contents)

### Structure générale

`GuideIA.md` commence par un titre `# ...` suivi du texte du guide. Il n'y a pas de délimiteur — le fichier entier est la zone parsée.

### Chapitres

Convention des niveaux de titres : voir `Methode.md`, section **Structure technique du guide** (source de vérité).

Un chapitre s'étend de son titre jusqu'au début du chapitre suivant (ou la fin du fichier). Le corps du chapitre est tout ce qui suit la ligne de titre, après suppression des espaces de tête et de queue.

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

Ces balises sont extraites par `parseGuide()` pour alimenter la vue de cohérence de `plan-editor.html`, et résolues en HTML par `render_html.js`.

## API de guide-parser.js
[up](#table-of-contents)

### Contexte d'exécution

`guide-parser.js` est conçu pour fonctionner dans deux contextes :

- **Node.js** — chargé via `require('./lib/guide-parser')` dans `render_html.js`
- **Navigateur** — chargé via `<script src="...">` dans `plan-editor.html`

Le fichier utilise un wrapper UMD minimal pour exporter dans les deux contextes.

### parsePlan(text)

Parse le contenu de `Plan.md` et retourne la liste des chapitres de la zone contenu.

**Paramètre :** `text` — contenu complet de `Plan.md` (string)

**Retour :** tableau d'objets chapitre, dans l'ordre d'apparition.

```javascript
[
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
]
```

- `num` : toujours une string (le numéro tel qu'il apparaît dans le fichier)
- `planContent` : texte brut de `#### Contenu`, vide si la section est absente
- `keywords` : tableau de strings, vide si `#### Mots-clés` est absent ou vide
- `figures` : tableau `{ id, caption }` — `caption` est la légende complète (lignes `>` concaténées)
- `encadres` : tableau `{ id, title }` — `title` est la première ligne de description

### parseGuide(text)

Parse le contenu de `GuideIA.md` et retourne un dictionnaire indexé par numéro de chapitre.

**Paramètre :** `text` — contenu complet de `GuideIA.md` (string)

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

- Clés : numéros de chapitres (strings)
- `content` : texte brut du chapitre (hors ligne de titre), vide si le chapitre n'existe pas
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

### Version 1.1 - Renvoi vers Methode.md pour la convention des titres
**Date:** 2026-06-05
**Reason:** La section "Chapitres" de GuideIA.md dupliquait la convention des niveaux de titres au lieu de pointer vers la source de vérité (`Methode.md`). Suppression de la duplication, renvoi explicite.

**Changes:**
- Format de GuideIA.md / Chapitres : bloc de spec remplacé par un renvoi vers `Methode.md`

---

### Version 1.0 - Création
**Date:** 2026-06-05
**Reason:** Aucune source de vérité documentée pour le format de Plan.md et GuideIA.md. Les deux parsers existants (render_html.js et plan-editor.html) avaient divergé silencieusement. Ce document fixe le format de référence et spécifie l'API de la librairie partagée guide-parser.js.

**Contenu initial :**
- Pourquoi une librairie partagée — historique de la divergence
- Format de Plan.md — délimiteur, chapitres, quatre sous-sections, syntaxe figures/encadrés/mots-clés/contenu
- Format de GuideIA.md — chapitres, balises def/ref
- API de guide-parser.js — parsePlan(), parseGuide(), fonctions internes, wrapper UMD
