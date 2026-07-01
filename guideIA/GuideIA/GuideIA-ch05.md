## 5. C'est quoi le contexte ?

Le mot est revenu plusieurs fois déjà. Il est temps de le définir, et surtout d'expliquer pourquoi il est indispensable.

### Ce que le modèle ne sait pas

Un LLM est entraîné sur tout. Il sait énormément de choses, mais il ne sait rien de vous. Pas votre situation, pas votre sujet, pas ce que vous attendez comme réponse. Sans contexte, il répond à tout le monde de la même façon : correctement, génériquement, et souvent à côté.

C'est le même problème qu'avec un nouveau collaborateur. Compétent, motivé, mais sans briefing, il produit du travail générique. Vous ne lui reprocheriez pas : vous ne lui avez rien dit.

Le {{def:mk:contexte}}, ou {{def:mk:prompt_étendu}}, c'est le briefing. Tout ce qu'on ajoute à votre question pour combler cet écart. Il sert deux usages distincts :

**Le contenu** : les objectifs du projet, votre situation, vos documents, le {{def:mk:persona}} que vous assignez au modèle. "Tu es un relecteur critique, ton rôle est de trouver les failles dans mon raisonnement, pas de me rassurer." Ce n'est pas de la flatterie : c'est une instruction qui reconfigure la génération.

**Le contenant** : la culture d'entreprise du projet. Style, ton, format, contraintes de comportement. "Pose des questions avant de proposer quoi que ce soit" contrecarre la tendance naturelle du modèle à agir avant de réfléchir, un biais d'action que vous apprendrez vite à reconnaître.

Exemple concret : vous préparez une réunion de gouvernance. Sans contexte, le modèle produit un ordre du jour générique. Avec le contenu (les membres du bureau, les décisions en suspens, le compte-rendu de la dernière séance) et le contenant (synthèse en cinq points, ton factuel, une proposition de décision par point) — vous avez un document utilisable en dix minutes.

{{def:enc:codage}}

### Du texte plat

Le modèle reçoit un grand texte à compléter, et c'est tout. Il ne peut pas hiérarchiser mécaniquement les éléments du contexte : on lui dit ce qui est important, et on espère qu'il comprend.

Même un collaborateur bien briefé peut se perdre si on lui donne trop d'informations en vrac. Le contexte joue un second rôle, pas seulement informer le modèle, mais conditionner la façon dont il traite votre question. On verra exactement pourquoi au chapitre suivant.

### Ce que l'opérateur vous offre

L'opérateur propose des outils pour enrichir le contexte de façon statique. Vous pouvez enregistrer des {{def:mk:instructions_utilisateur}} personnelles qui seront automatiquement insérées dans chaque prompt étendu — votre langue de travail, votre ton préféré, vos contraintes récurrentes.

{{def:fig:prompt-etendu-complet}}

Certains opérateurs vont plus loin. Claude propose par exemple la notion de **{{def:mk:projet}}** : un contexte partagé par un groupe de sessions, des instructions, des documents de référence, une mémoire commune. Utile pour un projet qui s'étale dans le temps, ou pour une équipe qui partage les mêmes conventions de travail.

Un projet peut inclure des fichiers de référence injectés dans chaque prompt. Pratique, mais chaque fichier ajouté est autant de tokens supplémentaires. Un contexte qui grossit trop dilue le focus. On verra pourquoi au chapitre suivant.

### La mémoire

Le modèle ne se souvient de rien entre les sessions. La {{def:mk:mémoire}} est une solution simple : du texte injecté dans le prompt système, clairement balisé. Le modèle sait qu'elle est là. Il peut proposer des mises à jour si la session apporte des éléments pertinents à retenir. 
Concrètement l'opérateur va conserver une mémoire (un texte) qui décrit ce qui est connu sur vous. Il va ensuite l'injecter après le prompt système en ajoutant deux instructions dans ce prompt systèmes:
- Il y a des informations sur l'utilisateur qui peuvent aider à répondre.
- Il est recommandé de proposer une mise à jour de ces informations au vu de la conversation.
L'opérateur va ensuite extraire la mise à jour qu'il ne va pas communiquer à l'utilisateur mais stocker dans la mémoire. On imagine comment le dosage des ces instructions peut influencer la mémoire qui se construit.

{{def:enc:memoire}}

### Le contexte dynamique

Jusqu'ici, le contexte est statique : défini à l'avance, injecté mécaniquement, légèrement modifié par la mémoire. Il existe une approche plus subtile : laisser le modèle aller chercher lui-même le contexte qui lui manque.

Au lieu d'un seul tour de génération par question, on en fait plusieurs. Une génération peut formuler une demande de contexte supplémentaire, que l'opérateur va chercher avant de relancer.

Exemple : *"Fait-il beau à Nice ?"* Les données d'entraînement du modèle sont passées, la météo du jour n'y figure pas. Le modèle sait pourtant des choses utiles : que cette question relève de la météo, que Nice est une ville, donc un lieu susceptible d'avoir une météo, et que la façon la plus pratique d'obtenir l'information est une recherche web. Sa réponse va donc contenir une demande : effectuer cette recherche. L'opérateur intercepte la réponse, et plutôt que de renvoyer à l'utilisateur: "tu n'as qu'à aller chercher sur le web toi-même" (un peu grossier) il exécute la requête web, reconstruit le prompt avec le résultat, et relance une nouvelle génération. Le modèle produit alors une réponse utile.

### Skills et MCP

La recherche web n'est qu'un exemple. Le {{def:mk:contexte_dynamique}} repose sur un concept plus général : des {{def:mk:outil}}s que le modèle peut solliciter.

Un **{{def:mk:skill}}** est un ensemble d'instructions décrivant une compétence à activer à la volée. Exemple : comment lire un fichier Excel. Le modèle reçoit le contenu brut du fichier et des instructions décrivant sa structure, il peut alors l'exploiter correctement.

Le concept ultime est le **{{def:mk:MCP}}**, *Model Context Protocol*, défini par Anthropic. Un MCP est une description fournie à l'opérateur pour accéder à un service externe : une liste de commandes disponibles et des instructions en langage naturel décrivant quand et comment les utiliser.

L'écosystème est vaste : lire et envoyer des emails, accéder à des fichiers et au cloud, interroger des bases de données, créer des factures dans un ERP, générer des modèles 3D...

{{def:enc:mcp-action}}
