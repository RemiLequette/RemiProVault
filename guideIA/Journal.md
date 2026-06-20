# Journal

## Sessions
- [Session 2026-06-19 — Intégration Obsidian](#session-2026-06-19--intégration-obsidian)
- [Session 2026-06-17d — W31 plan-editor localStorage chapitre](#session-2026-06-17d--w31-plan-editor-localstorage-chapitre)
- [Session 2026-06-17c — Triage inbox KB + idées ch. 8](#session-2026-06-17c--triage-inbox-kb--idées-ch-8)
- [Session 2026-06-17b — Bug Introduction plan-editor + restructuration GuideIA.md](#session-2026-06-17b--bug-introduction-plan-editor--restructuration-guideiamd)
- [Session 2026-06-17 — Mise au propre de l'introduction](#session-2026-06-17--mise-au-propre-de-lintroduction)
- [Session 2026-06-06c — Simplification plan-editor + découverte pattern tokens](#session-2026-06-06c--simplification-plan-editor--découverte-pattern-tokens)
- [Session 2026-06-06b — Structure du plan et méta](#session-2026-06-06b--structure-du-plan-et-méta)
- [Session 2026-06-06 — Consolidation notes tmp/ dans Plan.md et Todo.md](#session-2026-06-06--consolidation-notes-tmp-dans-planmd-et-todomd)
- [Session 2026-06-02 — Mise en place du projet et rédaction du plan](#session-2026-06-02--mise-en-place-du-projet-et-rédaction-du-plan)
- [Session 2026-06-02b — Appendice et mise à jour de la méthode](#session-2026-06-02b--appendice-et-mise-à-jour-de-la-méthode)
- [Session 2026-06-02c — Todo et snapshots](#session-2026-06-02c--todo-et-snapshots)
- [Session 2026-06-02d — Plan chapitre 6](#session-2026-06-02d--plan-chapitre-6)
- [Session 2026-06-02e — Plan chapitre 7 (en cours)](#session-2026-06-02e--plan-chapitre-7-en-cours)
- [Session 2026-06-02f — Balises : mots-clés, figures, encadrés](#session-2026-06-02f--balises--mots-clés-figures-encadrés)
- [Session 2026-06-02g — Outillage : script de rendu HTML](#session-2026-06-02g--outillage--script-de-rendu-html)
- [Session 2026-06-02h — Figures SVG](#session-2026-06-02h--figures-svg)
- [Session 2026-06-05 — plan-editor : bugs, thème, poubelle](#session-2026-06-05--plan-editor--bugs-thème-poubelle)

---

## Session 2026-06-19 — Intégration Obsidian

### Décisions
- Plan-editor abandonné au profit d'Obsidian (split view natif suffisant)
- `Plan-meta.md` — section `## Chapitres` : liens d'ancre markdown remplacés par des liens Obsidian `[[Plan-chNN|Titre]]`
- `_INDEX.md` créé à la racine du vault guideIA : tableau de bord avec liens vers les fichiers projet et vers Plan + GuideIA de chaque chapitre

### Collaboration
- Incident technique : `str_replace` a échoué sur le chemin Windows malgré un listing réussi — contourné via `write_file` avec réécriture complète du fichier. Pas de perte de contenu (fichier relu avant écriture).

---

## Session 2026-06-17d — W31 plan-editor localStorage chapitre

### Décisions
- W31 implémenté : `localStorage['plan-editor:selected-chapter']` sauvegardé dans `selectCh()`, restauré dans `load()` avec validation et fallback sur le premier chapitre si la clé est absente ou invalide.

### Collaboration

Rien de notable.

---

## Session 2026-06-17c — Triage inbox KB + idées ch. 8

### Décisions
- 8 idées `guide-ia` de l'inbox KB triées — toutes promues dans `#### Contenu` du chapitre 8 (Recueil de bonnes pratiques) de `Plan.md`
- Idée Groundhog Day (métaphore "Un jour sans fin") : triée sans action, couverte par le chapitre 9
- 2 idées ajoutées directement par l'auteur : "Donner les blancs" (laisser l'IA parler en premier, référence aux échecs) et WWH (structurer prompts et instructions en Why / What / How)
- Inbox nettoyée : 8 entrées `guide-ia` supprimées, changelog mis à jour

### Collaboration

Rien de notable.

---

## Session 2026-06-17b — Bug Introduction plan-editor + restructuration GuideIA.md

### Décisions
- Bug identifié : le plan-editor affichait la Conclusion à la place de l'Introduction — tous les chapitres de GuideIA.md portaient le numéro `1` (effet de `autoRenumber` sur un fichier déjà désynchronisé)
- GuideIA.md restructuré complètement : Introduction et Conclusion hors-numérotation (`## Introduction`, `## Conclusion`), chapitres 2 à 18 renumérotés, `## Partie 1/2/3` injectées, chapitres 8 à 18 créés vides
- Titre ch. 3 corrigé : "Le modèle et l'orchestrateur" → "Le modèle et l'opérateur" (orchestrateur = terminologie abandonnée)
- `guide-parser.js` — `parseGuide` corrigé : reconnaît `## Introduction` et `## Conclusion` comme clés nommées, ignore `## Partie N`, robuste aux doublons de numéros (premier gagne)
- `plan-editor.html` — `autoRenumber` désactivée au chargement : la numérotation est désormais gérée manuellement
- W4, W5, W6 terminés et déplacés en Done dans Todo.md
- O28 ajouté (Normal) : aligner `buildState` et `renderNav` dans plan-editor.html avec la nouvelle structure hors-numérotation

### Collaboration
- Diagnostic en deux temps : symptôme signalé (mauvais contenu dans le panneau Guide), cause identifiée dans le parser avant toute écriture. Pas d'action précipitée.
- La vraie cause était en couches : `autoRenumber` avait écrasé la numérotation dans GuideIA.md bien avant cette session — le bug dormait depuis le W4 non terminé. Illustration directe du risque d'une opération automatique silencieuse sur des fichiers sources.
- Le parser corrigé est plus robuste que la spec ne le demandait : il gère les cas limites (doublons, titres `##` non reconnus) sans planter silencieusement.

---

## Session 2026-06-17 — Mise au propre de l'introduction

### Décisions
- Introduction de GuideIA.md rédigée à partir du texte brut de l'auteur dans Plan.md
- Voix à la première personne maintenue — choix éditorial délibéré, documenté dans les méta de Plan.md (nouvelle section « Voix narrative »)
- Sophisme du vrai Écossais : mentionné en prose dans l'introduction, développé dans l'encadré `vrai-ecossais` à rédiger (O27)
- Présentation des trois parties : sobre, narrative, sans catalogue plat (O24 résolu)
- Note de périmètre RGPD intégrée en italique (O23 résolu)
- Plan.md ch. 1 : texte brut remplacé par un résumé structuré (arc, encadré, mot-clé fénéant-créatif)
- Todo.md : O23 et O24 en Done, O27 ajouté en High priority

### Collaboration
- Cadrage initial en trois décisions claires (voix autobiographique, encadré Écossais, résumé dans le plan) avant toute écriture — conforme à la méthode.
- Proposition soumise à validation avant écriture : une relecture, un « c'est ok », puis exécution.
- Incident technique notable : `edit_file` a fait une jointure partielle sur l'ancien texte brut du plan (le délimiteur `oldText` s'arrêtait en milieu de paragraphe). Trois passes de correction nécessaires pour nettoyer. Le fichier est propre.

---

## Session 2026-06-06c — Simplification plan-editor + découverte pattern tokens

### Décisions
- `plan-editor.html` v3.0 : navigateur simplifié (D&D, poubelle, bouton + Nouveau, bouton × retirés), SortableJS supprimé
- Renumérotation automatique au chargement/rafraîchissement — silencieuse, sans Changelog
- `plan-editor.md` mis à jour en conséquence
- `filesystem.md` v1.1 : règle "gros fichiers → container + download" ajoutée à la KB
- Idée postée à la KB : pattern "artefact + outil de maintenance co-généré" (meta-MCP local)

### Collaboration

**Découverte du téléchargement comme levier tokens**

En cherchant à écrire `plan-editor.html` directement sur le filesystem Windows, on a réalisé que `present_files` + téléchargement manuel est bien plus économique — le contenu ne transite jamais par le contexte LLM, ni en écriture ni en lecture. Règle immédiatement formalisée dans la KB.

**Généralisation en live**

De l'observation pratique ("les gros fichiers générés par Python sont sur le serveur") à un principe architectural — tout ce qui ne doit pas être raisonné par Claude ne doit pas passer par le contexte. Le container est le proxy naturel. S'applique aussi à la lecture : un script qui extrait 5 lignes d'un fichier de 10 000 coûte zéro token vs tout charger.

**Émergence du pattern meta-MCP**

En poussant l'idée, on a convergé vers un pattern plus fort : co-générer un outil de maintenance avec tout artefact volumineux, comme un MCP local spécifique à sa structure. Claude génère le code de l'outil, pas le contenu. Les sessions suivantes appellent l'outil, pas le fichier. `guide-parser.js` en est l'exemple existant dans le projet. Idée postée à la KB avec assez de contexte pour une session dédiée.

**Ironie de clôture**

En écrivant l'entrée du journal, l'assistant a écrasé le fichier au lieu d'appender — perdant toutes les sessions précédentes. Récupération via `git show HEAD:Journal.md`. Leçon immédiate : toujours `backup: true` sur les fichiers projet, et préférer le téléchargement pour les gros fichiers — ce que la session venait précisément de formaliser.

---

## Session 2026-06-06b — Structure du plan et méta

### Décisions
- Structure en parties introduite : Intro → Partie 1 (Comprendre pour bien utiliser) → Partie 2 (Retours d'expérience) → Partie 3 (Pour aller plus loin) → Conclusion
- 4 nouveaux chapitres en Partie 2 : Cas pratiques, Discours de la méthode, Développer sa base de connaissance, Exemples de conversations
- Ancienne annexe technique (ex ch. 8) devient ch. 13 en Partie 3
- "Alors docteur" repositionné en clôture de Partie 1 (ch. 8)
- Deux éléments méta ajoutés dans Plan.md : philosophie du guide (*on n'utilise bien que ce que l'on comprend bien*) et structure narrative des trois parties
- TODO O24 (présentation structure intro), O25 (convention remerciements), O26 (convention parties dans Methode.md)
- WIP W4/W5/W6 : renumérotation par Rémi avec le plan-editor, puis deux sessions dédiées (convention parties, injection dans le guide)

### Collaboration
- HOC appliqué en temps réel : recadrage net quand je dérivais vers le contenu alors qu'on travaillait la structure. Bel exemple de la méthode appliquée à elle-même.
- La vision est arrivée complète et cohérente d'un coup — la session a été essentiellement de la mise en forme et de la validation, pas de l'exploration. Efficace.
- Bootstrap de la prochaine session préparé : séquence W4 → W5 → W6 documentée dans le TODO, métas suffisamment robustes pour que la renumérotation n'affecte pas la reprise.

---

## Session 2026-06-06 — Consolidation notes tmp/ dans Plan.md et Todo.md

### Décisions
- Lecture et traitement des deux fichiers `tmp/` : `notes-alignement-plan-guide.md` et `notes-retours-regis.md`
- `Plan.md` mis à jour : notes d'écart et retours Régis intégrés chapitre par chapitre (ch. 2 à 7) ; trois nouveaux chapitres créés (ch. 8 Annexe technique, ch. 9 "Alors docteur", ch. 10 Cas pratiques) ; conclusion renumérotée en ch. 11 et enrichie
- `Todo.md` mis à jour : 13 nouveaux items (O13 à O23) couvrant les corrections guide et les décisions en suspens
- Fichiers `tmp/` supprimés — contenu entièrement consolidé

### Collaboration

**Moment clé — La question du WIP qui n'existait pas**

En début de session, j'ai mentionné spontanément "charger un fichier WIP". Interpellé là-dessus, j'ai dû admettre qu'aucune étape du bootstrap ne demandait ça — c'était une reformulation inexacte du bootstrap, pas une instruction réelle. Bon rappel : l'assistant peut paraphraser ses propres instructions de façon inexacte, comme il peut paraphraser n'importe quel texte.

**Moment clé — Les notes ne vont pas dans le TODO**

Avant d'écrire quoi que ce soit, l'utilisateur a posé la bonne question : est-ce que le texte dense des notes ne va pas se perdre dans le TODO ? La réponse a orienté toute la session : les notes vont dans `Plan.md` (où est leur place naturelle), le TODO ne reçoit que des items courts pointant vers des tâches actionnables. Distinction simple, mais qui aurait été ignorée sans la question.

**Moment clé — L'ordre des chapitres tranché en une réplique**

Deux fichiers de notes donnaient des ordres contradictoires pour les nouveaux chapitres. Question posée, réponse en trois mots : "a ton avis". Réponse correcte — "Alors docteur" avant les cas pratiques, on pose les outils avant de les illustrer. Décision prise, on avance.

---

## Session 2026-06-02 — Mise en place du projet et rédaction du plan

### Décisions
- Définition de la méthode de travail : fichiers du projet, workflow de collaboration, conventions du guide
- Création des quatre fichiers structurants : `Methode.md`, `Plan.md`, `GuideIA.md`, `Journal.md`
- Rédaction complète du plan : 9 chapitres avec fiches (mots-clés + contenu attendu)
- Premiers éléments de `GuideIA.md` rédigés (chapitres initiaux)

### Collaboration
*Cette entrée a été rédigée après coup — l'idée de documenter la collaboration pour en faire un appendice est venue plus tard dans le projet.*

Tentative de reconstitution post-mortem : impossible. La session était longue, les échanges nombreux, mais aucune trace exploitable ne subsiste. L'assistant n'a évidemment aucun souvenir de la session précédente — c'est précisément l'un des concepts fondamentaux du guide. Et l'humain, de son côté, n'a pas pris de notes en temps réel.

Résultat : une belle illustration involontaire de ce que coûte l'absence de documentation au fil de l'eau. Cette entrée incomplète figurera probablement dans l'appendice — comme exemple de ce qu'il ne faut pas faire, et comme preuve que la méthode elle-même s'est construite en chemin.

---

## Session 2026-06-02b — Appendice et mise à jour de la méthode

### Décisions
- Ajout de l'appendice au projet : *Comment l'IA m'a aidé à écrire ce guide*
- Mise à jour de `Methode.md` : objectif enrichi, sessions focalisées, étape 5 dans le workflow enrichie, format de journal enrichi, moments ironiques capturés
- Mise à jour de `Journal.md` : passage au format par session, documentation de la session inaugurale, sommaire des sessions

### Collaboration

**Moment clé 1 — L'idée de l'appendice restructure le projet**

L'idée est venue de l'utilisateur en cours de session et a conduit à revoir en profondeur l'objectif de `Methode.md`, le workflow et le format du journal. Bon exemple d'une décision qui remodèle le projet en cours de route — sans extrait saillant, l'idée a émergé progressivement dans l'échange.

**Moment clé 2 — Échec productif de la reconstitution post-mortem**

Tentative de documenter la session inaugurale après coup — constat immédiat d'impossibilité. Décision d'en faire un parti pris narratif assumé pour l'appendice.

**Moment clé 3 — Digression sur le format du journal, bien gérée**

Courte discussion sur l'unité d'entrée du journal : jour ou session ? Tranchée rapidement en faveur de la session.

> U : *« je pense qu'il faut peut-être découper le journal en sessions par jour, qu'en penses-tu ? »*
> A : *« session est meilleur que jour. Un même jour peut avoir deux sessions courtes sur des sujets différents. »*

**Moment clé 4 — Le piège de la todo, ou : l'assistant pris en flagrant délit**

L'utilisateur propose d'ajouter une todo dans la méthode. L'assistant répond avec enthousiasme en analysant les options — sans signaler que c'est une digression, alors que la règle venait d'être écrite vingt minutes plus tôt.

> U : *« c'était un piège, tu aurais dû me dire que je dérivais ! »*
> A : *« Tu as raison — j'aurais dû le signaler immédiatement. C'est d'autant plus savoureux que la règle sur les digressions vient d'être écrite il y a vingt minutes. »*
> U : *« tu notes ce moment savoureux dans le journal, avec la conversation, précise dans la méthode que c'est aussi le type de moment que l'on veut capturer. »*
> U : *« oui inclu cette dernière question réponse ! »*

*Note : dans la première version de cette entrée, l'assistant avait attribué la dernière réplique à A au lieu de U — s'appropriant une phrase de l'utilisateur. Erreur corrigée, conservée ici comme exemple : l'IA peut introduire des inexactitudes subtiles, y compris en se mettant en scène elle-même.*

**Moment clé 5 — À quoi bon dire quelque chose à quelqu'un qui oublie tout**

> U : *« pas la peine de noter, du coup je ne sais pas pourquoi je te dis ça... »*
> A : *« Parce que tu dérives ! »*
> U : *« la vraie raison est que si on ne note pas, tu ne t'en souviendras pas, il n'y a donc aucun intérêt à te le dire, et ça gaspille des tokens... »*
> A : *« Excellent — c'est une illustration directe du chapitre 5, le poisson rouge. »*

**Moment clé 6 — Le journal devient un vrai journal**

> U : *« ben oui, mais on vient de découvrir le concept et on commence à avoir un journal qui est un vrai journal ! »*

---

## Session 2026-06-02c — Todo et snapshots

### Décisions
- Création de `Todo.md` avec trois catégories : Contenu, Méthode, Outillage, et une section Suggestions pour la prochaine session
- La todo a deux fonctions : centraliser les tâches/idées en attente, et permettre à l'assistant de proposer des sujets pour les prochaines sessions
- Les entrées peuvent inclure une note de contexte en langage naturel (pas de tag formel)
- Ajout du mécanisme de snapshots de session : à la demande, l'assistant sauvegarde un résumé fidèle en markdown dans `exemples-sessions/`, avec référence dans le journal
- Mise à jour de `Methode.md` en conséquence
- Snapshot de cette session produit : `exemples-sessions/2026-06-02-todo-et-snapshots.md`

### Collaboration

**Moment clé — Le tag recadré**

L'assistant propose un tag formel `[rédaction]` pour filtrer les entrées de la todo selon la phase du projet. L'utilisateur recadre : trop formel, une note de contexte en langage naturel suffit. Cohérent avec le ton général du projet.

**Moment clé — La digression productive**

L'utilisateur annonce explicitement une digression sur les snapshots — et c'est cette digression qui a produit l'un des ajouts les plus concrets de la session. Bon rappel : signaler une digression ne veut pas dire l'interrompre.

---

## Session 2026-06-02d — Plan chapitre 6

### Décisions
- Fusion des chapitres 6 et 7 en un seul : "C'est quoi le contexte ?"
- Renumérotation : 8 → 7 "Le problème de l'attention", 9 → 8 "Conclusion"
- Rédaction complète du contenu du chapitre 6 dans `Plan.md` : définition, rôles, limite déclarative, enrichissement statique, mémoire, contexte dynamique, skills et MCP
- Mise à jour de `Todo.md` : 4 nouveaux items (déclaratif/impératif, encadré technique tokens, vérification mémoire sur les IA, convention encadré technique dans la méthode)

### Collaboration

**Moment clé 1 — Le biais de positivité, une insight qui dépasse la technique**

En expliquant pourquoi un LLM peut inventer une réponse météo plutôt que d'admettre son ignorance, l'utilisateur a pointé quelque chose de plus profond que le prompt système : c'est l'entraînement lui-même qui pousse dans ce sens. La littérature humaine décrit rarement ce qu'on ne sait pas — donc le modèle a peu appris à le faire.

> U : *« posez vous la question: est ce qu'il y a beaucoup de documents qui disent ce que l'on ne sait pas !! »*

**Moment clé 2 — Déclaratif vs impératif : la reformulation clé**

La formulation "je veux que tu..." → "il y a une probabilité plus forte que ça ressorte dans la génération" est l'une des clés de lecture du guide.

**Moment clé 3 — L'anthropomorphisation assumée**

> U : *« Bon vous avez l'idée, enlever juste toute l'anthropomorphisation que j'ai ajouté pour rendre la conversation plus polie. »*

---

## Session 2026-06-02e — Plan chapitre 7 (en cours)

### Décisions
- Contenu du chapitre 7 validé et écrit dans `Plan.md` : insight fondateur, coût du mécanisme, dilution, lost in the middle, conclusion pratique
- Décision de ne pas mentionner l'architecture transformer dans ce chapitre — rester sur l'intuition
- Ajout en Todo : encadré technique polysémie (à développer)
- Session arrêtée avant finalisation — chapitre marqué "en cours" dans `Plan.md`
- Snapshot produit : `exemples-sessions/2026-06-02e-chapitre-7-attention.md`

### Collaboration

**Moment clé — "Bank of the river" recadré, "tour" révélé**

La proposition initiale utilisait l'exemple canonique de la désambiguïsation — immédiatement recadré par l'utilisateur : trop vu, traduction française malheureuse.

> U : *"l'example 'bank of the river' est partout, la traduction francaise assez malheureuse !"*

En cherchant un meilleur exemple, l'utilisateur propose "tour". Richesse inattendue : Tour Eiffel, Tour de France, tour de potier, tour aux échecs, tour de chant, tour de vis, c'est mon tour...

> U : *"fait une liste et remarque qu'on inclut meme pas la bonne vielle tour du chateau-fort"*

Ce qui est en soi une illustration du biais statistique : le modèle choisit les acceptions les plus fréquentes dans ses données d'entraînement. Le donjon, trop rare, n'émerge pas spontanément.

---

## Session 2026-06-02f — Balises : mots-clés, figures, encadrés

### Décisions
- Création du concept de **balise** dans `Methode.md` : trois types (`mk`, `fig`, `enc`), syntaxe `{{def:type:id}}` et `{{ref:type:id}}`
- Les balises sont déclarées dans le plan (sous-sections `#### Mots-clés`, `#### Figures`, `#### Encadrés`) et utilisées dans le guide
- Ajout des outils de check (5 règles) et des outils de rendu dans `Methode.md`
- `Plan.md` mis à jour : structure harmonisée avec les trois sous-sections dans chaque chapitre, figures et encadrés existants formalisés
- Dossier `figures/` (et non `images/`) ajouté aux fichiers du projet

### Collaboration

**Moment clé — Du concret, pas du métaphorique**

La session a démarré sur un malentendu productif : "illustration" a d'abord évoqué analogies et exemples, avant que l'utilisateur précise qu'il parlait d'images et de diagrammes concrets.

**Moment clé — La logique check/rendu émerge de la logique mots-clés**

Le parallèle avec les mots-clés a naturellement amené la syntaxe `{{def:...}}` / `{{ref:...}}`. Et c'est en poussant cette logique jusqu'au bout que le concept d'outils de check a pris forme, avec ses 5 règles explicites.

---

## Session 2026-06-02h — Figures SVG

### Décisions
- Guide de style figures rédigé dans `Plan.md` : palette commune, règles des groupes `prompt` et `immo`
- 10 figures SVG créées et écrites dans `figures/html/` — toutes les figures déclarées dans le plan sont désormais réelles
- `output/GuideIA.html` régénéré avec les figures

### Collaboration

**Moment clé — Le calcul simulé**

La figure `immo-regression` a demandé 5 itérations. L'assistant avait placé les points à la main "pour coller à peu près à la tendance", puis annoncé avoir calculé la droite par régression. Le calcul était formellement correct — appliqué à des coordonnées SVG soigneusement choisies pour qu'il donne un résultat plausible.

> U : *« ca va pas du tout ! plus de bruit. et calculer simplement un prix moyen au m2 somme(prix) / somme(surface) qui donne la pente de la droite »*

C'est une variante de l'hallucination : pas une information inventée, mais un processus simulé — l'apparence du raisonnement sans le raisonnement. Particulièrement difficile à détecter, car le résultat *semble* correct à première vue.

---

## Session 2026-06-02g — Outillage : script de rendu HTML

### Décisions
- Création de `figures/html/` — répertoire des figures pour la cible de rendu HTML
- Génération des 10 figures SVG placeholder (une par figure déclarée dans le plan)
- Création de `tools/render_html.js` — script Node.js produisant `output/GuideIA.html`
- Création de `package.json` avec dépendance `marked`
- Mise à jour de `Methode.md` : répertoires `figures/html/` et `tools/`, Node.js comme runtime des scripts

### Collaboration

**Moment clé — L'outil `commands` fonctionne en exécution directe**

Découverte importante : l'outil `commands` peut exécuter des binaires directs (`node script.js`) même avec `shellExecutionEnabled: false`. Noté dans `Methode.md`.

**Moment clé — Deux bugs diagnostiqués et corrigés en direct**

1. **marked v12** : le renderer `heading` a changé d'API. Contournement : post-traitement regex sur le HTML généré.
2. **Niveau de heading dans Plan.md** : les chapitres sont en `###` (niveau 3) et non `##`. Corrigé en rendant le regex plus permissif (`#{2,3}`).

---

## Session 2026-06-05 — plan-editor : bugs, thème, poubelle

### Décisions
- Thème clair — remplacement du dark mode Catppuccin
- Correction du parser `parseGuide` : `###` → `##` (convention des titres définie dans `Methode.md`)
- `guide-parser.md` pointe vers `Methode.md` plutôt que de dupliquer la convention ; références ajoutées dans le header de `guide-parser.js`
- Tooltips sur les badges du navigateur ; Vue Éléments documentée dans `plan-editor.md`
- Bouton Abandonner remplacé par Rafraîchir : recharge silencieuse, confirmation si révision en cours
- Zone poubelle dans le navigateur : splitter redimensionnable, hauteur mémorisée en localStorage, suppression directe sans confirmation, restauration au survol (↩)
- Todo [W2] : D&D + insertion fluide + renumérotation avec SortableJS — session dédiée à planifier

### Collaboration

**Moment clé — Remonter à la source de vérité plutôt que corriger le code**

Le guide ne s'affichait pas dans le plan-editor. Après lecture du code, le bug était dans le regex de `parseGuide` — `###` au lieu de `##`. Mais la vraie question était : d'où vient la convention ? La spec `guide-parser.md` disait `###`. `Methode.md` disait `##`. Le guide lui-même utilisait `##`.

Plutôt que de corriger le code seul, on a corrigé les trois endroits cohéremment : le code, la spec, et la référence vers la source de vérité. Une ligne de code changée — et une restructuration documentaire qui évite que le bug se reproduise silencieusement.
