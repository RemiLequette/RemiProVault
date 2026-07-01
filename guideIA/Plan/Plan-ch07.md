# Plan — Chapitre 7

### 7. Recueil de bonnes pratiques

#### Mots-clés

- hallucination
- biais de positivité
- attention
- dilution
- boucle d'or

#### Figures

#### Encadrés

#### Contenu

**Introduction du chapitre :**

- On a failli appeler ce chapitre *Docteur, que fait-on ?*, posture clinique — et on y renonce, trop clinique.
- Référence à Astérix (*Le Combat des chefs*) : dans la salle d'attente d'Amnésix, un patient anonyme se prend pour un sanglier — Amnésix ne le guérit pas, il lui apprend à faire le beau.
- Clin d'œil « psychopathologie de la vie quotidienne » (Freud) : ces comportements sont triviaux et universels, pas des pathologies.
- Chute : on ne soigne pas ces limites structurelles — on développe des bonnes pratiques, comme dans n'importe quelle équipe, transmises et affinées par la tradition.

Chapitre pratique — boîte à outils face aux limites identifiées dans le guide.

**Pattern Pourquoi / Comment / Quoi** (retour Régis) : allers-retours fréquents entre
niveaux d'abstraction dans le travail avec une IA. Comment naviguer entre ces niveaux.
*(à recaser : probablement en ouverture du groupe C, comme grille de lecture des patterns)*

##### Pourquoi des bonnes pratiques fonctionnent

*Le fil cognitif : ces problèmes sont structurels, pas des bugs isolés — ils ressemblent
aux biais cognitifs humains, ce qui explique à la fois leur origine et leur remède.*

- Ce sont des problèmes assez humains finalement. Identifiés depuis longtemps par les
  psychologues et les philosophes — Système 1 / Système 2. *(vérifier redite avec
  ch01 où système1/système2/kahneman sont déjà posés en mots-clés — renvoyer plutôt que
  redéfinir)*
- Parce que ce n'est que des troubles cognitifs, ça nous affecte tous, tous les jours —
  ce ne sont pas des pathologies de l'IA, ce sont des biais ordinaires.
- Deux bonnes nouvelles :
  - on peut s'inspirer des techniques développées par les humains pour gérer ces biais
  - ces techniques sont elles-mêmes dans les données d'entraînement — donc le modèle
    peut les appliquer si on les convoque
- Tentative écartée : l'humour comme récompense pour cadrer le Système 1.
  Malheureusement ça ne marche pas. *(à creuser : pourquoi cette piste échoue)*
- Ces problèmes sont structurels : on ne peut que les pallier (au vrai sens du mot —
  remédier à un manque, sans le supprimer).

##### Les quatre problèmes structurels et leurs remèdes

###### Hallucinations
- Comment les détecter
- Comment limiter le risque

###### Biais de positivité
- Pourquoi l'IA dit rarement « je ne sais pas »
- Comment le forcer à l'exprimer

###### Attention / dilution
- Prompts courts
- Information critique en tête ou en queue (effet *lost in the middle*, voir ch06)

###### Amnésie
- *(remède à détailler — renvoi ch04/ch05 sur le poisson rouge et la session)*

##### Patterns de rédaction de prompt

*Boîte à outils transversale, indépendante des 4 problèmes ci-dessus — applicable à
toute rédaction de prompt ou d'instruction.*

- Définir son vocabulaire (Confucius — rectification des noms)
- On documente tout / RTFM
- Effet Streisand — mentionner ce qu'on veut éviter attire l'attention dessus ; éviter
  les formulations négatives dans les prompts et instructions
- Pas de référence en avant — ne pas introduire un concept pas encore expliqué (comme
  au rugby : pas de passe en avant) ; rend la lecture linéaire et autonome
- Pattern mot-clé — un mot-clé court et stable pour évoquer un concept sans le
  re-définir ; conversations courtes et précises, contexte partagé immédiat
- Principe ceinture et bretelles — deux mécanismes de sécurité qui se renforcent
  mutuellement valent mieux qu'un seul
- Donner les blancs — comme aux échecs, celui qui parle en premier a l'initiative ;
  laisser l'IA s'exprimer avant de lui soumettre ses propres idées pour ne pas biaiser
  la réponse
- WWH — structurer ses prompts et instructions en Why / What / How
- Interdire ou contraindre ? — le biais d'action : tendance à préférer une instruction
  active (« fais X ») à une interdiction (« ne fais pas Y ») *(à trancher : lequel
  marche mieux et pourquoi, lien possible avec l'effet Streisand ci-dessus)*

##### À recaser ou trancher

*(groupes E et D recasés — voir Plan-ch10.md et Plan-ch08.md)*

*(à compléter au fil des chapitres)*
