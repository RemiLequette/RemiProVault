# Plan — Méta

## Objectif

Définir le contenu attendu du guide guideIA : chapitres et ce qu'on veut y dire.

## Sommaire

- [Méta — hors guide](#méta--hors-guide)
- [Guide de style figures](#guide-de-style-figures)
- [Chapitres](#chapitres)
  - [Introduction](#introduction)
  - [Partie 1](#partie-1)
  - [Partie 2](#partie-2)
  - [Partie 3](#partie-3)
  - [Conclusion](#conclusion)

## Méta — hors guide

*Cette section ne fait pas partie du guide. Elle définit les conventions éditoriales propres à GuideIA : ton, public, style. Elle est lue par l'assistant IA en début de session de rédaction.*

### Public visé

Professionnels utilisant un assistant IA au quotidien, sans formation technique. Le guide suppose une curiosité intellectuelle, pas de connaissances en informatique ou en machine learning.

### Voix narrative

L'introduction et les parties expérientielles du guide sont écrites à la première personne. Ce choix est délibéré : la voix autobiographique engage le lecteur et pose la crédibilité de l'auteur. Elle n'est pas un style général du guide — les chapitres techniques restent impersonnels — mais elle est assumée là où elle apparaît.

### Ton de l'auteur

- **Humour pince-sans-rire**, jamais gratuit — toujours au service d'un point. Les formules sont soignées et mémorables ("les soldes aujourd'hui", "ado sous kétamine", "devoir à la maison").
- **Posture d'opinion assumée** : l'auteur défend des positions, anticipe les objections et les retourne. Il assume ses clichés s'ils sont utiles ("j'assume le cliché, il est équilibré").
- **Complicité avec le lecteur** : un "vous" complice, pas un lecteur à instruire. L'auteur parle *avec*, pas *à*.
- **Rythme syncopé** : phrases courtes qui claquent, suivies de développements. Accumulations et listes implicites. Transitions directes, presque abruptes.
- **Registre familier mais jamais relâché** : le mot juste prime sur l'élégance académique. Anglicismes bienvenus quand ils sont plus précis (*forgiving*, *big picture*, *drapeau rouge*).
- **Autodérision et ironie** : l'auteur ne se prend pas au sérieux, mais prend son sujet au sérieux.

### Ton général

- **Pédagogique et accessible** — jamais technique pour le plaisir d'être technique
- **Paragraphes courts et aérés**
- **Analogies et exemples concrets** — privilégier les situations du quotidien pour illustrer les concepts
- **Humour bienvenu, avec mesure** — uniquement au service de la compréhension, jamais gratuit
- **Concepts importants mis en valeur** — reformulation, exemple dédié, ou encadré
- **À éviter** : jargon technique, formulations trop formelles, condescendance

### Progression et dosage

Ces règles gouvernent la façon dont les concepts et les idées sont introduits au fil du texte :

- **Ne pas anticiper** — un concept qui sera expliqué plus tard ne s'introduit pas en passant. Même une mention fugace crée du bruit pour le lecteur qui ne dispose pas encore des clés.
- **Ne pas soulever une question que le lecteur ne se pose pas** — si la réponse est "non" ou "ça ne s'applique pas ici", ne pas poser la question. Le silence est plus propre.
- **Doser les mises en garde** — une mise en garde utile se glisse discrètement dans le texte, sans en faire un point de débat. L'objectif est qu'elle s'installe chez le lecteur, pas qu'elle déclenche une polémique.
- **Introduire sans insister** — un concept peut être évoqué brièvement à son premier passage, avec la promesse implicite que son rôle s'étoffera. Pas besoin de tout dire d'un coup.

### Conventions

- **Langue** : français — y compris les labels des figures
- **Fichier source** : `GuideIA/` — répertoire de fichiers par chapitre
- **Structure** : définie par les chapitres ci-dessous
- **Balises** : système `{{def:...}}` / `{{ref:...}}` décrit dans `Methode.md`

### Philosophie du guide

**On n'utilise bien que ce que l'on comprend bien.** Cette conviction est le fil directeur du guide : expliquer le fonctionnement interne des IA n'est pas une fin en soi, c'est la condition pour en tirer le meilleur parti et éviter les pièges.

### Structure narrative

Le guide est organisé en trois parties après l'introduction :

- **Partie 1** — forme un tout autonome et suffisant pour la plupart des lecteurs. Elle s'ouvre sur les grandes limitations (ce que les gens ont entendu comme faits ou légendes urbaines), explique la mécanique qui les cause, et se conclut sur les remèdes pratiques. Le lecteur qui comprend pourquoi ces limitations existent est bien mieux armé pour les contourner.
- **Partie 2** — retours d'expérience à la première personne. Cas pratiques, méthode, exemples de conversations.
- **Partie 3** — annexes techniques pour ceux qui veulent aller plus loin sous le capot. Le guide est complet sans elle.

### Note de collaboration

Ce projet est lui-même un exemple de collaboration humain-IA. La façon dont il se déroule — les échanges, les ajustements, les erreurs, les découvertes — constitue la matière première de l'appendice final du guide. Il faut donc documenter la collaboration au fil de l'eau, pas après coup.

---

## Guide de style figures

*Voir section "Rendu final du document" dans `Methode.md` pour les contraintes par cible de rendu.*

### Style général

- **Dimensions** : 700×420px, `viewBox="0 0 700 420"`
- **Fond** : blanc `#ffffff`
- **Typographie** : `font-family="sans-serif"` — labels 12-13px, titres/annotations 14-15px
- **Langue** : français pour tous les labels visibles

### Palette commune

| Rôle                             | Couleur                                   |
| -------------------------------- | ----------------------------------------- |
| Humain                           | `#6b7280` (gris)                          |
| opérateur / éléments structurels | `#0d9488` (teal)                          |
| Modèle / LLM                     | `#7c3aed` (violet)                        |
| Données / points                 | `#3b82f6` (bleu)                          |
| Outliers / alerte                | `#f97316` (orange)                        |
| Inadéquation (underfitting)      | `#ef4444` (rouge)                         |
| Surajustement (overfitting)      | `#7c3aed` (violet)                        |
| Zone interpolation               | fond `#d1fae5`, contour `#10b981` (vert)  |
| Zone extrapolation               | fond `#fee2e2`, contour `#ef4444` (rouge) |

---

### Groupe `prompt` — diagrammes de flux et de structure

Figures : `flux-opérateur`, `prompt-etendu-historique`, `prompt-etendu-complet`

**Nœuds (flux-opérateur)**

- Rectangles à coins arrondis (`rx="8"`), largeur fixe 140px, hauteur 48px
- Couleurs selon palette : humain gris, opérateur teal, modèle violet
- Label centré, blanc, 14px

**Flèches**

- Directionnelles, épaisseur 2px, couleur `#6b7280`
- Label court au-dessus (flux aller), en-dessous (flux retour), 12px, `#374151`

**Blocs de structure (`prompt-etendu-*`)**

- Empilement vertical, chaque couche rectangle plein, hauteur uniforme par type
- Couleurs par type de couche : prompt système teal `#0d9488`, instructions utilisateur bleu `#3b82f6`, historique gris `#9ca3af`, dernier prompt violet `#7c3aed`
- Label centré, blanc, 13px
- Indication visuelle de croissance de l'historique (hauteur variable ou annotation fléchée)

---

### Groupe `immo` — graphiques cartésiens

Figures : `immo-regression`, `immo-outliers`, `immo-deux-segments`, `immo-underfitting`, `immo-overfitting`, `immo-interpolation-extrapolation`

**Axes**

- x = surface (m²), y = prix (€)
- Labels courts, sans valeurs numériques précises (figures pédagogiques)
- Couleur axes : `#374151`, épaisseur 1.5px
- Graduation légère : tirets fins `#e5e7eb`

**Jeu de données**

- ~15 points, **même distribution dans toutes les figures du groupe**
- Cercles bleus `#3b82f6`, rayon 5px, `fill-opacity="0.75"`

**Éléments spécifiques par figure**

- `immo-regression` : droite teal `#0d9488`, épaisseur 2px ; lignes de lecture tiretées `#6b7280` pour un bien exemple
- `immo-outliers` : 2-3 outliers orange `#f97316`, contour distinct, rayon 6px
- `immo-deux-segments` : deux segments de droite, teal et violet, avec point de coupure visible
- `immo-underfitting` : droite rouge `#ef4444` qui ne suit pas la tendance ; annotation "modèle trop simple"
- `immo-overfitting` : courbe violette `#7c3aed` qui passe par tous les points, ondulée, trait 1.5px ; annotation "modèle trop complexe"
- `immo-interpolation-extrapolation` : zones colorées fond vert/rouge délimitant les données, labels "interpolation" / "extrapolation"

---

## Chapitres

**[[Plan-introduction|Introduction]]** *(hors partie)*

### Partie 1

1. [[Plan-ch02|Travailler avec une IA]]
2. [[Plan-ch03|Le modèle et l'opérateur]]
3. [[Plan-ch04|C'est quoi la génération ?]]
4. [[Plan-ch05|Le problème du poisson rouge]]
5. [[Plan-ch06|C'est quoi le contexte ?]]
6. [[Plan-ch07|Le problème de l'attention]]
7. [[Plan-ch08|Recueil de bonnes pratiques]]
8. [[Plan-ch09|Un jour sans fin]]

### Partie 2

9. [[Plan-ch10|Cas pratiques]]
10. [[Plan-ch11|Discours de la méthode]]
11. [[Plan-ch12|Développer sa base de connaissance]]
12. [[Plan-ch13|Exemples de conversations]]

### Partie 3

13. [[Plan-ch14|Les secrets du prompt système]]
14. [[Plan-ch15|Attention is all you need]]
15. [[Plan-ch16|L'apprentissage par renforcement]]
16. [[Plan-ch17|Le fantôme dans la machine]]
17. [[Plan-ch18|De la gouvernance des IA]]

**[[Plan-conclusion|Conclusion]]** *(hors partie)*
