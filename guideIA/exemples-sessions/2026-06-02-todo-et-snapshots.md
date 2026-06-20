# Session 2026-06-02 — Todo et snapshots

## Contexte
Session consacrée à deux ajouts à la méthode de travail : la todo list et les snapshots de session.

---

## Partie 1 : La todo list

**Utilisateur :** on va travailler sur l'idée d'une todo list dans la méthode

**Assistant :** Bonne idée. Voici ce que je propose comme concept :

Fichier `Todo.md` — une liste de tâches/idées en attente, organisée par catégories :
- Contenu — idées pour les chapitres
- Méthode — améliorations du workflow
- Outillage — scripts et outils à développer

Pour la gestion : n'importe lequel de nous deux peut proposer un ajout. Je formule, tu valides, j'écris.

Est-ce qu'on a besoin d'une 4e catégorie, par exemple Questions ouvertes ?

**Utilisateur :** il n'y a qu'une seule question, et oui ça rentre dans contenu.

---

**Assistant :** *(propose d'ajouter les outils déjà listés dans Methode.md)*

**Utilisateur :** l'outil pour la TOC et l'INDEX, c'est prématuré tant qu'on a pas écrit le guide mais autant s'en rappeler, peut-être ajouter sur une todo un contexte qui permettrait aussi de filtrer — par exemple celle qui ne sont valides que lorsque l'on commence à rédiger le guide lui-même.

**Assistant :** Bonne idée pour le contexte/filtre. Donc chaque entrée pourrait avoir un tag de phase, par exemple `[rédaction]`.

**Utilisateur :** un tag c'est trop formel, juste un contexte. Il y aura des moments où je te demanderai où on en est dans les TODO, tu me fais un petit aperçu, et je dédierai une session TODO où je commencerai par filtrer en fonction de l'avancement du projet. Tu pourrais aussi utiliser la TODO pour noter tes suggestions de prochains points à traiter pour me challenger en début de session.

> **Moment clé** : la proposition de tag formel `[rédaction]` a été recadrée — trop rigide. La solution retenue (note de contexte en langage naturel) est plus souple et cohérente avec le ton du projet. C'est aussi ici que la todo a pris une deuxième fonction : outil de suggestion pour l'assistant, pas seulement liste de tâches.

---

## Partie 2 : Les snapshots de session

**Utilisateur :** juste une petite digression maintenant, serais-tu capable de générer un snapshot fidèle d'une session ?

**Assistant :** Oui — je peux reconstituer un résumé structuré d'une session à partir de l'historique de conversation. La limite : je ne peux pas capturer ce qui s'est passé avant le début de la conversation courante.

**Utilisateur :** c'est bien de la session dont je parle. Je voudrais parfois être capable de faire des snapshots de sessions pour mon appendice d'exemples. On se met juste d'accord dans la méthode :
- l'utilisateur peut demander un snapshot de la session pour son appendice
- l'IA le sauve dans un dossier spécifique `exemples-sessions/` et met une référence dans le journal

**Assistant :** Deux précisions : format markdown ? Nommage du fichier ?

**Utilisateur :** oui markdown, identique à l'entrée dans le journal.

> **Moment clé** : la digression sur les snapshots était annoncée comme telle par l'utilisateur — et elle a débouché sur un ajout concret à la méthode. Bon exemple d'une digression productive qu'on aurait pu écarter trop vite.

---

## Fichiers produits
- `Todo.md` — créé
- `Methode.md` — mis à jour (Todo + Snapshots de session)
