 Méthode

## Objectif
Décrire la méthode de travail du projet guideIA : organisation des sessions, gestion des documents, et collaboration entre l'utilisateur et l'assistant IA.

## Sommaire
```insta-toc
---
title:
  name:
  level:
  center:
exclude:
style:
  listType:
omit:
levels:
  min:
  max:
---

# Table of Contents

- Objectif
- Sommaire
- Pourquoi cette méthode
- Fichiers du projet
    - Fichiers racine
    - Répertoire Plan/ — un fichier par chapitre
    - Répertoire GuideIA/ — un fichier par chapitre
    - Correspondance Plan / GuideIA
- Organisation des sessions
- Workflow de collaboration
- Todo
- Snapshots de session
- Conventions du guide final
- Structure technique du guide
    - Titres
    - Parties
- Guide de style
- Balises
    - Principe général
    - Déclaration dans le plan
    - Balises dans le guide
    - Fichiers des figures
    - Outils de check
    - Outils de rendu
- Encadrés et IA
- Rendu final du document
    - Cibles de rendu
        - HTML
        - PDF
        - Markdown
- Outillage
- Journal
- Idées futures
```

## Pourquoi cette méthode

Cette méthode et l'outillage associé sont conçus pour produire de la **littérature technique** : des documents destinés à être lus, maintenus, et enrichis dans la durée, en collaboration avec un assistant IA.

Par littérature technique on entend : guides pratiques, documentation de référence, manuels, tutoriels, ou tout document qui combine une structure formelle, un contenu rédactionnel soigné, et un système de balisage exploitable par des outils.

La méthode est **indépendante du sujet traité**. Elle définit l'organisation des sessions, le workflow de collaboration, le système de balises, et l'outillage — pas le ton ni le style éditorial, qui sont propres à chaque document et définis dans la section méta de son plan.

---

## Fichiers du projet

### Fichiers racine

| Fichier | Rôle |
|---|---|
| `Methode.md` | Méthode de travail et conventions du projet |
| `TODO.md` | Tâches en attente, idées, et suggestions de l'assistant |
| `Journal.md` | Journal de bord du projet |
| `Encadre.md` | Prompt de rédaction des encadrés par l'IA |
| `exemples-sessions/` | Snapshots de sessions sélectionnées pour l'appendice |
| `figures/` | Figures du guide (générées ou trouvées), référencées par identifiant |
| `figures/html/` | Figures pour la cible de rendu HTML (formats supportés par les navigateurs : SVG, PNG, JPEG...) |
| `tools/` | Scripts d'outillage (check, rendu, génération) — répertoire distinct du projet, chemin défini dans les instructions projet |
| `outils/` | Documentation des outils (specs, notes) — répertoire distinct du projet, chemin défini dans les instructions projet |

### Répertoire `Plan/` — un fichier par chapitre

Le plan est éclaté en fichiers indépendants, un par chapitre. Chaque fichier contient le contenu attendu du chapitre correspondant (mots-clés, figures, encadrés, contenu attendu).

| Fichier | Rôle |
|---|---|
| `Plan/Plan-meta.md` | Méta-données du plan : objectif, guide de style figures, liste des chapitres, conventions éditoriales (ton, public, style) propres à GuideIA. Ce fichier est chargé en début de toute session de rédaction. |
| `Plan/Plan-introduction.md` | Plan du chapitre Introduction |
| `Plan/Plan-ch01.md` … `Plan/Plan-ch17.md` | Plan de chaque chapitre (chapitres 1 à 17) |
| `Plan/Plan-conclusion.md` | Plan du chapitre Conclusion |

**Nommage des fichiers chapitre :**
- Introduction : `Plan-introduction.md`
- Chapitres numérotés : `Plan-chNN.md` (ex : `Plan-ch01.md`, `Plan-ch15.md`)
- Conclusion : `Plan-conclusion.md`

### Répertoire `GuideIA/` — un fichier par chapitre

Le guide final est éclaté en fichiers indépendants, miroir exact de la structure `Plan/`. Chaque fichier contient le texte rédigé du chapitre correspondant, avec les balises `{{def:...}}` et `{{ref:...}}`.

| Fichier | Rôle |
|---|---|
| `GuideIA/GuideIA-introduction.md` | Texte rédigé du chapitre Introduction |
| `GuideIA/GuideIA-ch01.md` … `GuideIA/GuideIA-ch17.md` | Texte rédigé de chaque chapitre (chapitres 1 à 17) |
| `GuideIA/GuideIA-conclusion.md` | Texte rédigé du chapitre Conclusion |

**Nommage des fichiers chapitre :**
- Introduction : `GuideIA-introduction.md`
- Chapitres numérotés : `GuideIA-chNN.md` (ex : `GuideIA-ch01.md`, `GuideIA-ch15.md`)
- Conclusion : `GuideIA-conclusion.md`

### Correspondance Plan / GuideIA

Chaque fichier `Plan/Plan-chNN.md` a son miroir `GuideIA/GuideIA-chNN.md`. Les outils de check et de rendu traitent ces paires conjointement pour vérifier la cohérence des balises et produire le document final assemblé.

Chaque fichier de travail (`Methode.md`, `Plan/Plan-meta.md`) doit maintenir une TOC simple juste après son objectif : une liste des sections avec une référence (ancre markdown).

## Organisation des sessions


Les sessions doivent être **focalisées** : un sujet par session, clairement défini au départ. Si la conversation dérive vers un autre sujet, l'assistant le signale immédiatement et propose de traiter la digression dans une session séparée.

**Dégradation du contexte :** une session longue qui couvre plusieurs sujets devient moins fiable — les décisions prises tôt deviennent floues. Signaux d'alerte : l'assistant répète quelque chose déjà décidé, contredit un choix antérieur, ou produit quelque chose qui ne colle pas à l'objectif. Quand cela arrive : reposer l'objectif, résumer les décisions prises, et continuer.

**Fin de session — mot-clé `clôture` :**
C'est l'utilisateur qui signale la fin de session en prononçant le mot-clé **`clôture`**. Ce signal déclenche obligatoirement l'écriture du journal :
1. L'assistant propose une entrée de journal (décisions + collaboration)
2. L'utilisateur valide ou ajuste
3. L'assistant écrit l'entrée dans `Journal.md`

L'assistant n'écrit jamais le journal de sa propre initiative — il attend le mot-clé.

## Workflow de collaboration

1. **L'utilisateur indique le sujet de travail**

2. **L'assistant pose ses questions en premier** — avant de partager ses idées, l'utilisateur laisse l'assistant s'exprimer librement sur le sujet : connaissances, patterns, analogies. Une question ouverte bien placée fait souvent émerger des idées qui affinent l'objectif avant tout travail. Partager ses idées trop tôt risque de biaiser les réponses de l'assistant. Vient ensuite un échange contradictoire : chacun propose, challenge, reformule.

3. **Aucun fichier n'est modifié avant un go explicite de l'utilisateur** — on conçoit dans la conversation d'abord, on écrit sur disque ensuite.

4. **L'assistant écrit le fichier concerné**

5. **Si ce n'est pas écrit, ça n'existe pas** — une conversation n'est pas un enregistrement. Insights, décisions, exceptions découvertes en cours de session : tout ce qui ne finit pas dans un fichier est perdu à la clôture. Le réflexe : dès que quelque chose vaut la peine d'être gardé, l'écrire avant de fermer — todo, note, mise à jour de la méthode. La forme importe peu. La trace, oui.

6. **Noter dans le journal** tout moment remarquable de la session :
   - Identifier les moments clés — ils peuvent porter sur la méthode de travail autant que sur le contenu (plan, rédaction du guide) : reformulation utile, erreur corrigée, échange ayant amélioré le contenu, limite rencontrée
   - Capturer aussi les moments amusants ou ironiques — particulièrement quand la méthode est mise en défaut ou illustrée de façon inattendue : ce sont souvent les plus parlants pour l'appendice
   - Proposer si pertinent un extrait de la session illustrant le moment clé
   - Valider avec l'utilisateur le contenu de chaque moment clé avant de l'écrire
   - Garder un nombre limité de moments clés — en avoir trop est un signal que la session a dérapé

**HOC — Hiérarchie des préoccupations :** ne pas mélanger les niveaux dans une même réponse. Deux échecs fréquents : (1) noyer un problème structurel au milieu de remarques mineures — le problème critique passe inaperçu ; (2) répondre à une question stratégique avec des détails d'implémentation — la question de fond reste sans réponse. Règle : traiter le niveau de préoccupation le plus élevé en premier, complètement, avant de descendre.

**Option — écriture directe par l'utilisateur :** l'utilisateur peut écrire directement dans un fichier du projet, puis prévenir l'assistant. L'assistant relit, reformule sa compréhension, et on valide ensemble avant toute écriture finale. Ce n'est pas le mode par défaut — l'utilisateur choisit selon les moments.

## Todo

Dossier : `TODO/ITEMS/` — un fichier `.md` par item, convention KB `todo-list.md`.

La todo centralise tout ce qui n'est pas traité dans la session en cours : tâches à faire, idées sur le contenu ou la méthode, outils à développer. Elle sert aussi à l'assistant pour noter ses suggestions de sujets à traiter lors des prochaines sessions.

**Format :** chaque item est un fichier `TODO/ITEMS/<titre>.md` avec YAML front matter (`Status`, `importance`, `effort`, `type`). Template : `TODO/ITEM template.md`.

**Gestion :** n'importe lequel des deux peut proposer un ajout. L'assistant formule, l'utilisateur valide, l'assistant écrit le fichier item.

**Aperçu :** à la demande, l'assistant liste `TODO/ITEMS/` et filtre selon le statut ou la priorité. Les sessions dédiées permettent de la passer en revue selon l'avancement du projet.

## Snapshots de session

L'utilisateur peut demander à tout moment un snapshot de la session en cours, destiné à l'appendice du guide.

- L'assistant génère un résumé fidèle de la session en markdown
- Le fichier est sauvegardé dans `exemples-sessions/` avec le même nommage que l'entrée de journal correspondante : `YYYY-MM-DD-[sujet].md`
- L'assistant ajoute une référence au snapshot dans l'entrée de journal de la session

## Conventions du guide final

- **Structure en répertoires** : `Plan/` pour les fiches chapitre, `GuideIA/` pour les textes rédigés — un fichier par chapitre dans chaque répertoire
- Langue : français
- Structure définie dans `Plan/Plan-meta.md` (liste des chapitres) et les fichiers `Plan/Plan-chNN.md` (contenu attendu)
- Chaque document produit avec cette méthode devrait inclure une section **Style du document** dans son plan — partis pris éditoriaux spécifiques au sujet et au public visé, qui complètent ou amendent le guide de style général.

## Structure technique du guide

### Titres
- `#` — titre principal du document
- `##` — parties
- `###` — chapitres
- `####` — sous-sections

### Parties
- Les parties regroupent les chapitres en blocs thématiques.
- Numérotées : **Première Partie**, **Deuxième Partie**, **Troisième Partie**.
- Sans titre pour le moment — les noms viendront plus tard.
- Dans le guide (`GuideIA.md`) : niveau `##`, ex. `## Première Partie`
- Dans le plan (`Plan.md`) : niveau `###` dans la section `## Chapitres`, ex. `### Première Partie`
- Dans les fiches de contenu (après le délimiteur) : même niveau `###` avant les fiches des chapitres de la partie.
- Les parties ne sont pas numérotées dans le sommaire du guide — elles y apparaissent comme séparateurs visuels.

## Guide de style

Le ton, le style, les règles de progression et les conventions éditoriales de GuideIA sont définis dans la section **`## Méta — hors guide`** de `Plan/Plan-meta.md`.

Cette séparation est intentionnelle : `Methode.md` décrit une méthode réutilisable pour tout document de littérature technique. Les choix éditoriaux sont propres à chaque document et vivent dans son plan.

## Balises

Les balises permettent de déclarer dans le plan des éléments nommés (mots-clés, figures, encadrés), puis de les définir et référencer dans le guide. Les outils lisent conjointement les paires `Plan/Plan-chNN.md` / `GuideIA/GuideIA-chNN.md` pour vérifier la cohérence et produire le document final assemblé.

### Principe général

Les mots-clés servent deux usages distincts : la navigation pour le lecteur (index alphabétique avec lien vers la définition) et les renvois internes entre chapitres (balises `{{ref:...}}`).

Chaque balise a :
- un **type** (`mk`, `fig`, `enc`)
- un **identifiant** unique dans l'ensemble du document
- une **description** dans le plan (texte libre)
- une **définition** dans le guide (une occurrence exactement) — `{{def:type:id}}`
- zéro ou plusieurs **références** dans le guide — `{{ref:type:id}}`

### Déclaration dans le plan

Les balises sont déclarées dans des sous-sections dédiées de chaque chapitre : `#### Mots-clés`, `#### Figures`, `#### Encadrés`. Le texte du plan mentionne les balises de façon libre ("utiliser la figure X ici", "voir encadré Y").

**Mots-clés** — liste simple, un identifiant par ligne :
```
#### Mots-clés
- anthropomorphisme
- hallucination
```

**Figures** — bloc descriptif, aussi détaillé que nécessaire pour guider la création ou la recherche :
```
#### Figures

> 🖼️ **figure** `flux-orchestrateur`
> Diagramme du flux simplifié : l'humain envoie un prompt,
> l'orchestrateur le transmet au modèle, le modèle retourne
> une complétion, l'orchestrateur affiche la réponse à l'humain.
```

**Encadrés** — bloc descriptif du contenu à rédiger dans le guide :
```
#### Encadrés

> 📦 **encadré** `polysemie`
> La polysémie
> Pourquoi le langage est intrinsèquement
> ambigu, avec exemples concrets (tour, sel...).
```

La **première ligne** après l'en-tête est le **titre court** de l'encadré — c'est lui qui apparaît dans la table des encadrés du rendu. Les lignes suivantes sont la description pour guider la rédaction dans le guide.

### Balises dans le guide

**Définition** — là où l'élément est introduit (une seule fois, dans le bon chapitre) :
```
{{def:mk:hallucination}}
{{def:fig:flux-orchestrateur}}
{{def:enc:polysemie}}
```

**Référence** — renvoi depuis n'importe quel autre endroit du texte :
```
{{ref:mk:hallucination}}
{{ref:fig:flux-orchestrateur}}
{{ref:enc:polysemie}}
```

Les outils de rendu remplacent `{{def:...}}` et `{{ref:...}}` par les formes appropriées (terme mis en valeur, numéro de figure, renvoi "voir encadré X") et génèrent les index en fin de document.

### Fichiers des figures

Les figures sont stockées dans `figures/`, nommées par leur identifiant (ex : `flux-orchestrateur.svg`). Elles sont produites soit par recherche, soit par génération avec un outil dédié, lors d'une session consacrée à cet effet.

### Outils de check

Les outils de check vérifient la cohérence entre `Plan.md` et `GuideIA.md`. Ils doivent être exécutés avant tout rendu.

**Règles vérifiées :**

1. **Unicité des identifiants dans le plan** — un identifiant de balise apparaît dans les sous-sections d'un seul chapitre (d'un seul fichier `Plan-chNN.md`). Un identifiant présent dans deux chapitres est une erreur.

2. **Au moins une définition dans le guide** — chaque identifiant déclaré dans le plan a au moins une balise `{{def:...}}` dans le fichier `GuideIA-chNN.md` correspondant. Zéro définition (balise manquante) est une erreur. Plusieurs définitions sont autorisées — chacune génère une entrée distincte dans l'index des mots-clés.

3. **Définition dans le bon chapitre** — la balise `{{def:...}}` d'un identifiant se trouve dans le fichier `GuideIA-chNN.md` miroir du fichier `Plan-chNN.md` où il est déclaré.

4. **Références valides** — toute balise `{{ref:...}}` dans n'importe quel fichier `GuideIA-chNN.md` pointe vers un identifiant déclaré dans le plan. Une référence vers un identifiant inexistant est une erreur.

5. **Cohérence du type** — le type utilisé dans `{{def:...}}` et `{{ref:...}}` correspond au type déclaré dans le plan (`mk`, `fig`, `enc`).

### Outils de rendu

Les outils de rendu produisent le document final à partir de `Plan.md`, `GuideIA.md` et `figures/`. Ils supposent que le check a été exécuté sans erreur.

**Transformations appliquées — cible HTML :**

- `{{def:mk:id}}` → terme en ***italique gras*** avec ancre HTML (`id="mk-{id}"`)
- `{{ref:mk:id}}` → terme en *italique* avec lien vers l'ancre (`#mk-{id}`)
- `{{def:fig:id}}` → insertion de la figure depuis `figures/html/id.*`, avec légende et numéro
- `{{ref:fig:id}}` → "voir figure N" avec lien vers la figure
- `{{def:enc:id}}` → insertion du bloc encadré tel que rédigé dans le guide, avec numéro
- `{{ref:enc:id}}` → "voir encadré N" avec lien vers l'encadré

**Navigation HTML :**
- La TOC inclut en fin de liste (après un séparateur) des liens vers les trois sections d'index
- Chaque `h2` de chapitre, chaque titre de section d'index, et chaque entrée des index (mots-clés, figures, encadrés) comportent un lien de retour vers la TOC (`↑`)

**Index générés en fin de document :**
- Index des mots-clés (alphabétique, avec lien vers la définition)
- Table des figures (numéro, légende, lien)
- Table des encadrés (numéro, titre, lien)

## Encadrés et IA

Les encadrés du guide sont rédigés par l'assistant IA sous la direction de l'auteur.

Le prompt de rédaction est défini dans `Encadre.md` (racine du projet). Il doit être chargé en début de toute session consacrée à la rédaction d'encadrés.

Le processus est en deux étapes obligatoires : cadrage (proposition de plan, validation humaine) puis rédaction finale. L'assistant ne rédige jamais l'encadré final au premier tour.

## Rendu final du document

Le guide est rédigé dans `GuideIA.md` — source unique, lisible en markdown par un humain. Le rendu produit des versions destinées à la diffusion à partir de cette source, en appliquant les balises et en insérant les figures.

Les contraintes sur les figures varient selon la cible de rendu — c'est pourquoi le guide de style figures (dans `Plan.md`) doit anticiper ces contraintes dès la création.

### Cibles de rendu

#### HTML

*Cible prioritaire — la plus simple à produire.*

- Stockage des figures : `figures/html/` — un fichier par identifiant (ex : `flux-orchestrateur.svg`, `immo-regression.png`)
- Le script de rendu insère les figures en `<img src="...">` avec légende générée depuis le plan
- Script : `render_html.js` (Node.js) — assemble tous les fichiers `GuideIA/GuideIA-chNN.md` dans l'ordre du plan, applique les balises, et produit `output/GuideIA.html` — chemin défini dans les instructions projet

#### PDF

*À définir.*

#### Markdown

*À définir.*

## Outillage

Les fichiers `GuideIA/GuideIA-chNN.md` doivent rester lisibles en markdown par un humain.
L'objectif final est de produire des versions PDF, HTML ou autres via des scripts qui assemblent et transforment ces fichiers.

Les conventions de balisage doivent être sans ambiguïté pour éviter les erreurs dans ces scripts.
Elles ne doivent pas entrer en conflit avec la syntaxe markdown standard.

Les scripts d'outillage sont écrits en **JavaScript** et exécutés avec **Node.js**.

Les scripts d'outillage et leur documentation sont dans des répertoires distincts du projet guideIA. Leurs chemins sont définis dans les instructions projet.

**Exécution depuis une session :** l'outil `commands` peut lancer des scripts directement même sans accès shell complet. C'est le moyen privilégié pour tester les scripts sans quitter la session.

Des outils seront développés pour automatiser les tâches mécaniques :
- **plan-editor** : outil de navigation et d'édition du plan et du guide — spec dans `outils/plan-editor.md`
- **Check** : vérification de la cohérence entre `Plan.md` et `GuideIA.md` (voir section Balises)
- **Rendu** : transformation des balises et génération des index (voir section Balises)
- Génération de la TOC du guide
- Génération du sommaire des sessions dans `Journal.md`
- Export vers PDF, HTML, etc.

Le LLM se concentre sur le contenu sémantique ; les tâches structurelles (assemblage, numérotation, index, rendu) sont déléguées aux outils.

> **Chantier à explorer** : préparer également des présentations à partir du guide — slides avec images et animations. Format et outillage à définir.

## Journal

Fichier : `Journal.md`  
Format d'une entrée :
```
## Session YYYY-MM-DD — [sujet court]

### Décisions
- ...

### Collaboration
- ...
```

Le fichier commence par un sommaire des sessions avec liens vers chaque entrée. Ce sommaire est destiné à être généré automatiquement par un outil.

Chaque entrée doit couvrir deux dimensions :
- **Décisions** : ce qui a été produit ou arrêté durant la session
- **Collaboration** : moments notables de l'échange humain-IA (reformulation qui a débloqué quelque chose, direction corrigée, limite de l'IA constatée, approche qui a bien fonctionné), avec extraits si pertinent

Ces notes alimenteront directement l'appendice du guide.


---

## Idées futures

- **Versions multilingues** : si le guide rencontre un succès suffisant, envisager des traductions (anglais en priorité). La méthode de collaboration humain-IA pourrait s'appliquer directement à la traduction.
