# Plan — Chapitre 6

### 6. C'est quoi le contexte ?

#### Mots-clés

- contexte
- prompt étendu
- instructions utilisateur
- projet
- déclaratif
- mémoire
- skill
- MCP
- contexte dynamique
- outil

#### Figures

> 🖼️ **figure** `prompt-etendu-complet`
> Schéma de la structure complète du prompt étendu avec toutes les couches :
> [prompt système] → [instructions utilisateur] → [historique de la session] → [dernier prompt].
> Variante enrichie de la figure prompt-etendu-historique.

#### Encadrés

> 📦 **encadré** `tokens-speciaux`
> Tokens spéciaux et génération
> → Déplacé vers l'annexe technique.

#### Contenu

**Définition**
Le contexte, c'est tout ce qu'on ajoute au prompt initial pour former le prompt étendu
envoyé au moteur. On en a déjà vu deux exemples : le prompt système (géré par
l'opérateur) et l'historique de la session. Le contexte a plusieurs rôles :
contrôler le style de la réponse, orienter les priorités de génération, et donner
de la mémoire au modèle.

*Note : la section déclaratif/impératif est à supprimer du guide (décision de session).
La supprimer aussi du plan — ne pas développer ce concept ici.*

*Note : remplacer la référence à l'encadré `tokens-speciaux` dans le guide par un renvoi
vers l'annexe technique.*

*Note : ajouter un petit exemple narratif autour de l'email — sur le modèle de l'exemple
météo du contexte dynamique. Renforce un concept important pour ceux qui n'ont pas encore
généralisé. Rapide, en "racontant une histoire".*

*Note : positionner ici le concept "boucle d'or" (Goldilocks) — niveau de détail optimal
dans un prompt. Ni trop vague (le modèle improvise), ni trop contraint (on perd la valeur
de l'IA). À placer après l'explication du contexte déclaratif, comme règle pratique issue
du fonctionnement du modèle.*

**Pourquoi "contexte" est bien choisi**
Le langage humain est par essence ambigu et dépendant du contexte.
"Tu peux me passer le sel ?" n'est pas une question sur vos capacités physiques.
C'est précisément ce mécanisme — l'attention au contexte — qui a débloqué la technologie
des LLM. On y reviendra dans le chapitre suivant.

**La limite fondamentale : tout ça reste du texte plat**
Le LLM ne se regarde pas travailler. Au final, il reçoit un grand texte à compléter
— et c'est tout. Il ne peut pas hiérarchiser mécaniquement les éléments du contexte :
on lui dit ce qui est important, et on espère qu'il comprend.

**Enrichir le contexte : ce que l'opérateur vous offre**
L'opérateur pose des outils pour enrichir le contexte de façon statique.
Vous pouvez enregistrer des instructions personnelles qui seront systématiquement
insérées dans le prompt étendu. Utiliser la figure prompt-etendu-complet ici.

Certains opérateurs vont plus loin. Claude propose par exemple la notion de
**projet** : un contexte spécifique partagé par un groupe de sessions — des instructions,
des documents de référence, une mémoire commune.

Un projet peut aussi inclure des **fichiers de référence** — documentations, notes,
exemples — qui sont injectés dans le prompt étendu. Utile quand leur contenu est
fondamental pour la session. Mais attention : ce sont autant de tokens supplémentaires,
et un contexte qui grossit trop risque de diluer le focus de la session.

**La mémoire**
L'opérateur peut gérer une mémoire — globale ou par projet. Elle est injectée dans
le prompt, clairement balisée. Le prompt système est enrichi en conséquence : il indique
au modèle que le prompt contient une mémoire, et l'invite à proposer des modifications
si la session apporte des éléments pertinents à mémoriser.

Avantage : une mémoire se construit entre les sessions. Inconvénient : on ne maîtrise
pas toujours bien ce qui s'y accumule — un peu comme les cookies publicitaires.

> **TODO** : vérifier sur les différents assistants IA comment consulter et contrôler sa mémoire.

Conseil pour un usage professionnel : restreindre la mémoire au niveau projet, voire
s'en passer. La réserver aux usages plus personnels.

**Le contexte dynamique**
Jusqu'ici, le contexte était statique : défini à l'avance, injecté mécaniquement.
Il existe une approche plus subtile : laisser le LLM lui-même choisir le contexte
dont il a besoin. Concrètement, au lieu d'un seul tour de génération par prompt
utilisateur, on en fait plusieurs. Une génération peut inclure dans sa réponse des
requêtes de contexte supplémentaire, qui seront injectées dans le prompt suivant.

Exemple narratif : "Fait-il beau à Nice ?"
La date d'entraînement du modèle est passée — la météo du jour n'y figure pas.
Avec de la chance, le modèle répond honnêtement qu'il ne sait pas. Mais il peut
aussi inventer une réponse vraisemblable — c'est le biais de positivité.

Pourtant le modèle sait des choses utiles : que la question relève de la météo,
que la météo s'applique à un lieu, que Nice est une ville, et que le plus pratique
est de faire une recherche web. Sa réponse va donc contenir une demande : effectuer
cette recherche et lui communiquer le résultat.

Voir encadré tokens-speciaux (annexe technique) pour la mécanique sous-jacente.

**Skills et MCP : le contexte dynamique généralisé**
La recherche web n'est qu'un exemple. Le contexte dynamique repose sur un concept
plus général : des **outils** que le LLM peut solliciter pour obtenir du contexte.

Un **skill** est un ensemble d'instructions avec un contexte d'applicabilité, qui
ajoute une compétence à la volée.

Le concept ultime est le **MCP** — Model Context Protocol, défini par Anthropic.
Un MCP est une description fournie à l'opérateur pour accéder à un service
externe. Il expose :

- une liste de commandes disponibles (recherche web, lecture de fichier, lecture d'emails...)
- des instructions en langage naturel décrivant dans quelles conditions utiliser cet outil et comment formuler chaque commande

L'industrie a réagi avec enthousiasme : on trouve maintenant des centaines de MCP
permettant, en langage naturel, de lire et envoyer des emails, d'accéder à des
fichiers et au cloud, d'interroger des bases de données, de créer des factures dans
un ERP, de générer des modèles 3D...
