# Render HTML

Doc de l'outil `tools/render_html.js` — script Node.js qui produit `output/GuideIA.html` à partir des fichiers source du projet.

*Document type: Process spec*

*Language: French — ce document cible le projet guideIA francophone.*

## Quick Start

Charger avant toute modification de `render_html.js` ou de son comportement.
Couvre le pipeline de traitement, les fichiers en entrée et en sortie, les dépendances, et l'usage.
Ne couvre pas le format des fichiers source — voir `outils/guide-parser.md`.
Ne couvre pas le contenu éditorial — voir `Plan-meta.md` et `Methode.md`.

## Keywords
render-html, rendu, HTML, pipeline, balises, index, figures, assemblage, output

## Table of Contents

1. [Rôle](#role)
2. [Usage](#usage)
3. [Fichiers](#fichiers)
4. [Pipeline](#pipeline)
5. [Balises — transformations HTML](#balises--transformations-html)
6. [Index générés](#index-generes)
7. [CSS et mise en page](#css-et-mise-en-page)
8. [Dépendances](#dependances)
9. [Index](#index)

## Rôle
[up](#table-of-contents)

`render_html.js` est l'outil de rendu principal du projet. Il assemble tous les fichiers chapitres en un document HTML unique, autonome et auto-contenu : CSS inline, figures SVG inline ou raster en base64, pas de dépendance externe au moment de la lecture.

Le fichier produit `output/GuideIA.html` est la cible de diffusion prioritaire du guide.

## Usage
[up](#table-of-contents)

```bash
cd C:\Users\RemiLequette\Development\tools\guideIA-tools
node tools/render_html.js
```

Depuis une session Claude, via l'outil `commands` :

```
node tools/render_html.js
```

Le script affiche sa progression sur stdout et termine par un résumé (nombre de mots-clés, figures, encadrés traités).

## Fichiers
[up](#table-of-contents)

### Entrées

| Chemin | Rôle |
|--------|------|
| `G:\My Drive\...\guideIA\Plan\Plan-introduction.md` | Plan du chapitre Introduction |
| `G:\My Drive\...\guideIA\Plan\Plan-ch01.md` … | Plans des chapitres numérotés |
| `G:\My Drive\...\guideIA\Plan\Plan-conclusion.md` | Plan du chapitre Conclusion |
| `G:\My Drive\...\guideIA\GuideIA\GuideIA-introduction.md` | Texte rédigé du chapitre Introduction |
| `G:\My Drive\...\guideIA\GuideIA\GuideIA-ch01.md` … | Textes rédigés des chapitres numérotés |
| `G:\My Drive\...\guideIA\GuideIA\GuideIA-conclusion.md` | Texte rédigé du chapitre Conclusion |
| `figures/html/` | Figures SVG ou raster, nommées par identifiant |

Les chapitres numérotés sont découverts dynamiquement par lecture du répertoire `Plan/`, triés numériquement.

### Sorties

| Chemin | Contenu |
|--------|---------|
| `output/GuideIA.html` | Document HTML complet, autonome |

## Pipeline
[up](#table-of-contents)

Le script traite les sources en huit étapes séquentielles :

1. **Assemblage Plan** — lecture et concaténation de tous les fichiers `Plan/Plan-*.md` dans l'ordre (introduction → chapitres numérotés → conclusion)

2. **Assemblage Guide** — même ordre, fichiers `GuideIA/GuideIA-*.md`

3. **Parse du plan** — extraction du registre des éléments nommés (mots-clés, figures, encadrés) via `guide-parser.js`. Chaque élément est indexé par identifiant avec son chapitre d'origine et sa métadonnée (légende pour les figures, titre pour les encadrés).

4. **Résolution des balises** — remplacement des `{{def:...}}` et `{{ref:...}}` dans la source guide assemblée par leur équivalent HTML. Deux passes : la première assigne les numéros d'ordre (figures et encadrés dans l'ordre de leur première `{{def:...}}`), la seconde remplace.

5. **Conversion Markdown → HTML** — via la librairie `marked`.

6. **Post-traitement HTML** — injection d'attributs `id` sur les titres `h1`/`h2`/`h3` (slugification du texte), ajout d'un lien `↑` de retour TOC sur chaque `h2`.

7. **Construction des éléments de navigation** — TOC (table des matières avec liens internes + séparateur vers les sections d'index), index des mots-clés, table des figures, table des encadrés.

8. **Assemblage et écriture** — le `h1` est extrait du corps et placé avant la TOC ; les index sont ajoutés après un `<hr>` en fin de document ; le tout est emballé dans le template HTML avec CSS inline.

## Balises — transformations HTML
[up](#table-of-contents)

| Balise source | Transformation HTML |
|---------------|---------------------|
| `{{def:mk:id}}` | `<strong><em><span id="mk-id">terme</span></em></strong>` |
| `{{ref:mk:id}}` | `<em><a href="#mk-id">terme</a></em>` |
| `{{def:fig:id}}` | Bloc `<figure id="fig-id">` avec figure inline et `<figcaption>` |
| `{{ref:fig:id}}` | `<a href="#fig-id" class="fig-ref">voir figure N</a>` |
| `{{def:enc:id}}` | Ancre `<span id="enc-id">` + label `Encadré N` |
| `{{ref:enc:id}}` | `<a href="#enc-id" class="enc-ref">voir encadré N</a>` |

Le terme affiché pour les mots-clés est l'identifiant avec les tirets bas remplacés par des espaces (ex : `fenetre_de_contexte` → `fenetre de contexte`).

**Figures — résolution des fichiers :**

Le script cherche dans `figures/html/` un fichier nommé `<id>.<ext>` dans l'ordre : `.svg`, `.png`, `.jpg`, `.jpeg`, `.webp`, `.gif`.

- SVG : inliné directement dans le HTML (l'entête XML et les déclarations inutiles sont supprimées)
- Raster : encodé en base64 et inséré en `<img src="data:...">` — le fichier HTML reste autonome

Si aucun fichier n'est trouvé, un bloc `[figure manquante : id]` est affiché à la place.

## Index générés
[up](#table-of-contents)

Trois sections sont ajoutées en fin de document, après un séparateur horizontal :

- **Index des mots-clés** — tableau alphabétique (`localeCompare` français), chaque terme lié à son ancre `#mk-id` avec indication du chapitre de définition
- **Table des figures** — tableau numéroté dans l'ordre d'apparition, chaque entrée liée à son ancre `#fig-id`
- **Table des encadrés** — même structure, avec le titre court de l'encadré

Chaque section d'index est omise si elle est vide (aucun élément du type correspondant dans le document).

La TOC inclut un séparateur visuel puis des liens vers les trois sections d'index en fin de liste.

## CSS et mise en page
[up](#table-of-contents)

Le CSS est inline dans le fichier HTML généré — aucune feuille de style externe.

Partis pris de mise en page :

- Largeur de colonne fixe à 740px, centrée — optimisé pour la lecture longue
- Typographie sérif (Georgia) pour le corps — convention des livres et guides imprimés
- Monospace (`Courier New`) pour les blocs de code
- Palette neutre : texte `#1a1a1a`, accent `#2a6496` (bleu discret), fonds gris très clair pour les encadrés et les figures
- Encadrés : fond `#f4f8fb`, bordure gauche bleue — visuellement distincts sans être intrusifs
- Figures : fond `#f8f8f8`, bordure légère, légende en italique gris

Les variables CSS sont définies dans `:root` — un seul endroit à modifier pour changer la palette ou la largeur.

### Sauts de page et séparateurs de chapitres

Le rendu combine deux mécanismes complémentaires selon le contexte de lecture :

**À l'écran** : un séparateur visuel (`.chapter-break`) est injecté avant chaque `h2` à partir du deuxième. Il se présente comme un filet fin avec un espace généreux (4em) — chaque chapitre est clairement délimité sans interrompre le défilement.

**À l'impression / export PDF** : un `page-break-before: always` est appliqué sur chaque `h2`. Chaque chapitre commence sur une nouvelle page. Le séparateur visuel est masqué en `@media print` pour éviter le doublon.

Le premier `h2` du document (Introduction) est exclu du saut de page via `h2:first-of-type { page-break-before: avoid }` — il suit directement la TOC.

## Dépendances
[up](#table-of-contents)

| Dépendance | Usage | Installation |
|------------|-------|--------------|
| `marked` | Conversion Markdown → HTML | `npm install marked` |
| `guide-parser.js` | Parsing Plan et Guide | Librairie locale (`tools/lib/`) |

Aucune autre dépendance externe. Le script utilise uniquement `fs`, `path`, et `marked`.

## Index

| Terme | Occurrences |
|-------|-------------|

## Changelog

### Version 1.0 - Création
**Date:** 2026-06-29
**Raison:** Doc manquante pour `render_html.js`. L'outil existe depuis juin 2026 mais n'était pas documenté dans `outils/`. Création en remplacement de la doc `plan-editor.md` (outil retiré).

**Contenu initial :**
- Rôle, usage, fichiers en entrée/sortie
- Pipeline en 8 étapes
- Tableau des transformations de balises
- Résolution des figures (SVG inline, raster base64)
- Index générés et leur structure
- CSS et partis pris de mise en page
- Dépendances
