## 6. C'est quoi le contexte ?

On a déjà croisé ce mot plusieurs fois. Il est temps de le définir proprement.

### Définition

Le {{def:mk:contexte}} — ou {{def:mk:prompt_étendu}} — c'est tout ce qu'on envoie au modèle, au-delà de votre question immédiate. On en a déjà vu deux exemples : le prompt système, ajouté par l'opérateur, et l'historique de la session. Le contexte a plusieurs rôles : contrôler le style de la réponse, orienter les priorités, et donner de la mémoire artificielle au modèle.

### Pourquoi "contexte" est bien choisi

Le langage humain est par essence ambigu. *"Tu peux me passer le sel ?"* n'est pas une question sur vos capacités physiques. Comprendre ce que veulent dire les mots dépend des autres mots autour d'eux — c'est précisément ce mécanisme qui a débloqué la technologie des LLM. On y reviendra au chapitre suivant.

### Une limite fondamentale : du texte plat

Le LLM ne se regarde pas travailler. Il reçoit un grand texte à compléter — et c'est tout. Il ne peut pas hiérarchiser mécaniquement les éléments du contexte : on lui dit ce qui est important, et on espère qu'il comprend.

Point crucial : le contexte est toujours **déclaratif, jamais impératif**. Quand vous écrivez *"réponds en trois points"*, comprenez : *"la probabilité que la réponse contienne trois points est plus forte"*. Ce n'est pas un ordre exécuté — c'est une intention formulée. Certaines formulations sont plus efficaces que d'autres, et on verra comment en tirer parti.

### Ce que l'opérateur vous offre

L'opérateur propose des outils pour enrichir le contexte de façon statique. Vous pouvez enregistrer des {{def:mk:instructions_utilisateur}} personnelles qui seront automatiquement insérées dans chaque prompt étendu.

{{def:fig:prompt-etendu-complet}}

Certains opérateurs vont plus loin. Claude propose par exemple la notion de **{{def:mk:projet}}** : un contexte partagé par un groupe de sessions — des instructions, des documents de référence, une mémoire commune.

Un projet peut inclure des fichiers de référence — documentations, notes, exemples — injectés dans chaque prompt. Utile quand leur contenu est fondamental. Mais attention : ce sont des tokens supplémentaires, et un contexte qui grossit trop risque de diluer le focus. On retrouve ici le conseil du chapitre précédent : sessions courtes et centrées.

### La mémoire

L'opérateur peut gérer une {{def:mk:mémoire}} — globale ou par projet. Elle est injectée dans le prompt, clairement balisée. Le prompt système indique au modèle qu'une mémoire est disponible, et l'invite à proposer des mises à jour si la session apporte des éléments pertinents. En résumé : le modèle peut prendre des notes pour les prochaines sessions.

Avantage : une mémoire qui se construit dans le temps. Inconvénient : on ne maîtrise pas toujours bien ce qui s'y accumule — un peu comme les cookies publicitaires.

Pour un usage professionnel : restreindre la mémoire au niveau projet, voire s'en passer. La réserver aux usages plus personnels.

### Le contexte dynamique

Jusqu'ici, le contexte était statique : défini à l'avance, injecté mécaniquement. Il existe une approche plus subtile : laisser le modèle lui-même choisir le contexte dont il a besoin.

Concrètement, au lieu d'un seul tour de génération par question, on en fait plusieurs. Une génération peut inclure des demandes de contexte supplémentaire, que l'opérateur va chercher avant de relancer la génération.

Exemple : *"Fait-il beau à Nice ?"* Les données d'entraînement du modèle sont passées — la météo du jour n'y figure pas. Le modèle sait pourtant beaucoup de choses utiles : que cette question relève de la météo, que la météo s'applique à un lieu, que Nice est une ville, et que la façon la plus pratique d'obtenir la météo en temps réel est de faire une recherche web. Il sait probablement quel service appeler.

Sa réponse va donc contenir une demande : effectuer cette recherche et lui communiquer le résultat. L'opérateur exécute, récupère l'information, reconstruit le prompt avec le résultat, et relance la génération. Le modèle produit alors une réponse utile.

*(Tout l'anthropomorphisme de cet exemple est assumé pour la lisibilité — il n'y a bien sûr ni intention ni mémoire d'une demande à l'autre.)*

### Skills et MCP

La recherche web n'est qu'un exemple. Le {{def:mk:contexte_dynamique}} repose sur un concept plus général : des {{def:mk:outil}}s que le modèle peut solliciter.

Un **{{def:mk:skill}}** est un ensemble d'instructions décrivant une compétence à activer à la volée. Exemple : comment lire un fichier Excel. Le modèle reçoit le contenu brut du fichier et des instructions décrivant sa structure — il peut alors l'exploiter correctement.

Le concept ultime est le **{{def:mk:MCP}}** — *Model Context Protocol*, défini par Anthropic. Un MCP est une description fournie à l'opérateur pour accéder à un service externe. Il expose une liste de commandes disponibles et des instructions en langage naturel décrivant quand et comment les utiliser.

L'industrie a réagi avec enthousiasme : on trouve maintenant des centaines de MCP permettant, en langage naturel, de lire et envoyer des emails, d'accéder à des fichiers et au cloud, d'interroger des bases de données, de créer des factures dans un ERP, de générer des modèles 3D...
