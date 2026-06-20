# TODO

Backlog du projet guideIA — tâches, idées de contenu, idées sur la méthode, et outils à développer.

*Language: French — this document targets a French-speaking project.*

## Quick Start

Backlog léger du projet guideIA.
Contient les tâches en attente, idées de contenu, idées sur la méthode, et outils à développer.
Sert aussi à noter les suggestions de l'assistant pour les prochaines sessions.
Charger en début de session pour prendre connaissance des tâches en attente.

## Keywords
todo, backlog, guideIA, tâches, idées, contenu, méthode, outillage

## High priority

- [ ] [O1] Finaliser le plan du chapitre 7 | Exemples, articulation avec ch. 5 et 6, lien "lost in the middle"

## Normal

- [ ] [O3] Décider si les tokens sont développés dans le chapitre 5 ou dans un encadré dédié ailleurs
- [ ] [O4] Trouver une mise en valeur adaptée pour le concept déclaratif/impératif | Encadré, analogie, ou autre forme éditoriale (chapitre 6)
- [ ] [O5] Développer l'encadré technique | Tokens spéciaux et génération token par token (mentionné dans le chapitre 6)
- [ ] [O6] Vérifier sur les différents assistants IA comment consulter et contrôler sa mémoire | Claude, ChatGPT, Gemini...
- [ ] [O7] Développer l'encadré technique | La polysémie — pourquoi le langage est intrinsèquement ambigu (mentionné dans le chapitre 7)
- [ ] [O14] Corriger le guide ch. 1 | Introduire LLM et équivalents grand public (ChatGPT, Gemini) — absent de la version actuelle
- [ ] [O16] Supprimer déclaratif/impératif du guide ch. 6 | Décision de session — passage présent dans le guide, à retirer
- [ ] [O18] Réintroduire Monsieur Jourdain au ch. 4 | "Sans le savoir, je viens de construire un modèle" → "Comme Monsieur Jourdain faisait de la prose..."
- [ ] [O19] Ajouter exemple narratif email au ch. 6 | Sur le modèle de l'exemple météo — renforce le concept de contexte dynamique
- [ ] [O21] Ajouter paragraphe prospectif en conclusion | Retour Régis — perspective historique, rythme d'innovation, espoirs et craintes. Remplace "Bonne collaboration"
- [ ] [O30] Outillage — script journal-append.js | Insère une entrée en tête des sessions dans Journal.md sans relire le fichier entier. Appelé via l'outil `commands`. Évite la réécriture complète du journal à chaque clôture.
- [ ] [O31] Outillage — script todo-add.js | Ajoute un item dans la section appropriée de TODO.md via l'outil `commands`. Même logique que journal-append.js.

## Low priority

- [ ] [O8] Explorer la possibilité de produire des présentations à partir du guide | Slides avec images et animations — format et outillage à définir
- [ ] [O25] Convention remerciements dans Methode.md | Définir la bonne pratique : placement (fin de conclusion vs section séparée), forme, qui citer — pour ne pas être ingrat dans le futur
- [ ] [O29] Explorer rendu PDF de qualité | LaTeX écarté (trop austère). Pistes : Pandoc + WeasyPrint (scriptable, CSS typographique, supporte widows/orphans) ou Affinity Publisher/InDesign (manuel, qualité impression). À explorer quand le contenu est plus avancé. [effort: M]

## WIP

## Done

- [x] [W31] plan-editor — mémoriser le chapitre en cours | localStorage `plan-editor:selected-chapter` — sauvegarde dans `selectCh()`, restauration dans `load()` avec fallback sur ch.1. [effort: XS]
- [x] [D26] Ajouter la convention "parties" dans Methode.md | Structure technique et markdown pour les parties (niveau au-dessus des chapitres) — titre, numérotation, rendu HTML
- [x] [D27] Rédiger l'encadré `vrai-ecossais` | Sophisme du vrai Écossais appliqué à l'IA — déclaré dans Plan.md ch. 1, à rédiger dans GuideIA.md (session dédiée écriture encadrés)
- [x] [D1] Librairie guide-parser.js | Extraire le parsing de Plan.md et GuideIA.md de render_html.js vers tools/lib/guide-parser.js, partagée par render_html.js et plan-editor.html. Session dédiée. [effort: L]
- [x] [D2] Phrase périmètre RGPD en introduction | Ajoutée en italique en fin d'introduction, avant la présentation des parties
- [x] [D3] Présentation sobre des trois parties en introduction | Arc narratif annoncé sans catalogue plat — résolu dans la mise au propre de l'introduction
- [x] [D4] Injection des parties dans le guide | ## Partie 1, ## Partie 2, ## Partie 3 injectées dans GuideIA.md avec les chapitres vides 8 à 18. Structure complète alignée sur Plan.md.
- [x] [D5] Convention parties dans Methode.md | Structure markdown des parties documentée dans Methode.md (## Partie N dans GuideIA.md, ### Partie N dans Plan.md). Appliquée.
- [x] [D6] Renumérotation des chapitres dans GuideIA.md | Tous les ## 1. écrasés corrigés : Introduction hors-numéro, ch. 2 à 18 renumérotés, Conclusion hors-numéro. autoRenumber désactivée dans plan-editor. parseGuide corrigé dans guide-parser.js.
- [x] [D7] plan-editor — réordonnancement D&D et insertion fluide | Trois choses liées : (1) ligne + au survol entre chapitres pour insérer à la position souhaitée, (2) D&D dans la liste pour réordonner, (3) D&D depuis la poubelle pour restaurer à une position précise. Renumérotation automatique en séquence au Save, répercutée dans Plan.md et GuideIA.md. SortableJS (cdnjs). [effort: L]
- [x] [D9] Définir la convention "encadré technique" dans Methode.md | Usage, format, place dans le guide
- [x] [D15] Étoffer "Valider" au ch. 2 | Trop court — ajouter exemples concrets : vérifier les sources citées, croiser avec le bon sens, tester les affirmations factuelles
- [x] [D17] Introduire le biais de positivité au ch. 4 | Après l'explication de l'interpolation — troisième grand problème structurel avec hallucinations et attention
- [x] [D20] Positionner la "boucle d'or" au ch. 6 | Niveau de détail optimal dans un prompt (Goldilocks) — après l'explication du contexte déclaratif
- [x] [D23] Ajouter phrase RGPD/données privées en introduction | Délimiter le périmètre : ce guide traite du fonctionnement et de l'utilisation, pas des aspects légaux ou de confidentialité
- [x] [D28] Aligner plan-editor.html avec la structure Introduction/Conclusion | buildState fait la correspondance plan/guide par numéro — ne couvre pas les clés 'introduction' et 'conclusion' de parseGuide. renderNav cherche l'Introduction avec ch.num === '1'. À corriger pour que l'éditeur affiche correctement ces deux sections hors-partie. [effort: S]
- [x] [D1] Outil de relecture et coopération | Un outil permettant de relire le guide section par section avec l'assistant — navigation dans le texte, annotations, propositions de reformulation, validation collaborative. Format et interface à définir ensemble en session. [effort: XL]
- [x] [D10] Générateur de TOC du guide | À faire une fois la rédaction du guide commencée
- [x] [D11] Générateur d'index à partir des balises {{mot-clé}} | À faire une fois la rédaction du guide commencée
- [x] [D13] Export du guide | Vers PDF, HTML, etc.

## Index

| Term | Occurrences |
|------|-------------|

## Changelog

### Version 7.4 - O30 et O31 ajoutés
**Date:** 2026-06-19
**Reason:** Scripts Node.js pour journal et todo — éviter la réécriture complète des fichiers en clôture de session.

**Changes:**
- Normal : O30 ajouté (journal-append.js), O31 ajouté (todo-add.js)

---

### Version 7.3 - W31 Done
**Date:** 2026-06-17
**Reason:** W31 terminé — localStorage `plan-editor:selected-chapter` implémenté.

**Changes:**
- WIP : W31 déplacé en Done

---

### Version 7.2 - W4 ajouté + encadrés ch.2
**Date:** 2026-06-17
**Reason:** Session encadrés ch. 2 — deux encadrés rédigés et intégrés (theorie-de-lesprit, systeme1-systeme2). W4 ajouté pour la mémorisation du chapitre en cours dans plan-editor.

**Changes:**
- WIP : W4 ajouté (plan-editor localStorage chapitre en cours)

---

### Version 7.1 - O29 ajouté
**Date:** 2026-06-17
**Reason:** Exploration LaTeX — écarté (trop austère). O29 ajouté en Normal pour explorer Pandoc + WeasyPrint comme alternative.

**Changes:**
- Normal : O29 ajouté

---

### Version 7.0 - W4/W5/W6 Done + O28 ajouté
**Date:** 2026-06-17
**Reason:** Session plan-editor — correction du bug Introduction/Conclusion dans le plan-editor. W4, W5, W6 terminés et déplacés en Done. O28 ajouté pour le résidu buildState/renderNav.

**Changes:**
- WIP : W4, W5, W6 déplacés en Done
- Done : W4, W5, W6 ajoutés avec descriptions à jour
- Normal : O28 ajouté (aligner buildState et renderNav avec la structure hors-numérotation)

---

### Version 6.0 - Consolidation notes tmp/ + nouveaux chapitres
**Date:** 2026-06-06
**Reason:** Intégration des notes tmp/notes-alignement-plan-guide.md et tmp/notes-retours-regis.md.
Renumérotation des chapitres suite à l'ajout de ch. 9 "Alors docteur" et ch. 10 "Cas pratiques".

**Changes:**
- High priority : O2 mis à jour (ch. 8 Conclusion → ch. 9 et 10 nouveaux) ; O13 ajouté (trancher {{def:mk:prompt}})
- Normal : O14 à O23 ajoutés (corrections guide ch. 1/2/4/6, boucle d'or, biais de positivité, conclusion)

---

### Version 5.0 - W3 ajouté + batch thème/démarrage
**Date:** 2026-06-05
**Reason:** Session plan-editor — batch de corrections : thème todo-tool appliqué, sélection automatique ch.1 au démarrage/refresh. W3 toggle HTML read-only ajouté en Low priority.

**Changes:**
- W3 ajouté en Low priority

---

### Version 4.0 - W2 Done
**Date:** 2026-06-05
**Reason:** W2 terminé — déplacé de WIP vers Done.

**Changes:**
- W2 déplacé en Done avec description mise à jour

---

### Version 3.0 - Reformatage pour todo-tool
**Date:** 2026-06-05
**Reason:** Installation du todo-tool — items reformatés avec séparateur titre | description, gras retiré des titres.

**Changes:**
- Items avec titre en gras convertis au format `Titre | Description`
- Items sans titre structurés avec un titre court séparé par ` | `
- Fichier précédent sauvegardé sous TODO-old.md

---

### Version 2.0 - Mise en conformité avec la convention todo-list
**Date:** 2026-06-05
**Reason:** Mise en conformité avec `conventions/todo-list.md` — ajout Quick Start, Keywords, Index, Changelog, restructuration en sections High/Normal/Low/Done.

**Changes:**
- Ajout Quick Start, Keywords, Index, Changelog
- Contenu migré dans les sections de priorité
- Items prioritaires placés en High priority

---

### Version 1.0 - Création
**Date:** inconnue
**Reason:** Backlog initial du projet guideIA.
