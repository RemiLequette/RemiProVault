# Plan — Chapitre 5

### 5. C'est quoi le contexte ?

#### Objectifs

Le lecteur comprend pourquoi le contexte est nécessaire : la connaissance du LLM est générique — entraîné sur tout, il ne sait rien de votre situation particulière. Le contexte est le pont entre ce savoir générique et votre demande concrète. Il sert deux usages distincts : définir le **contenu** (votre situation, votre sujet, vos documents, le persona que vous assignez au modèle) et contrôler le **contenant** (style, ton, format, contraintes — y compris le comportement du modèle : "pose des questions avant de répondre" contrecarre par exemple la tendance naturelle du LLM à agir avant de réfléchir).

Le lecteur distingue contexte statique (ce que vous apportez : couches du prompt étendu, instructions personnelles, projet, mémoire) et contexte dynamique (ce que le modèle va chercher lui-même quand le statique ne suffit pas — exemple météo, puis MCP comme généralisation).

Ces deux dimensions contenu/contenant sont illustrées par un exemple concret (email professionnel délicat) et par un encadré dédié au cas du codage.

#### Mots-clés

- contexte
- prompt étendu
- instructions utilisateur
- projet
- mémoire
- persona
- skill
- MCP
- contexte dynamique
- outil

#### Figures

> 🖼️ **figure** `prompt-etendu-complet`
> Structure complète du prompt étendu avec toutes les couches
> Schéma de la structure complète du prompt étendu avec toutes les couches :
> [prompt système] → [instructions utilisateur] → [historique de la session] → [dernier prompt].
> Variante enrichie de la figure prompt-etendu-historique.

#### Encadrés

> 📦 **encadré** `codage`
> Le contexte en action : l'exemple du code
> Cas du développeur : le contenu c'est le problème à résoudre et le code existant ;
> le contenant c'est les conventions du projet (langage, style, nommage).
> Illustration que les deux dimensions sont souvent aussi importantes l'une que l'autre.

> 📦 **encadré** `memoire`
> Jusqu'où ça va ?
> Les opérateurs proposent tous des mécanismes de mémoire, plus ou moins contrôlables.
> Tendance des fournisseurs à enrichir ces mécanismes : aller chercher dans d'autres
> conversations, construire des profils utilisateur par inférence.
> Motivation implicite : rendre le système plus familier, réduire la friction.
> Anecdote : une IA a inféré "grand-père" à partir d'une recherche sur la transmission
> patrimoniale aux petits-enfants — faite en réalité pour le compte de sa maman.
> Risque en contexte pro : on ne contrôle pas ce qui s'y accumule, des informations
> fausses peuvent s'y glisser silencieusement.
> Conseil : restreindre la mémoire au niveau projet, voire s'en passer.

> 📦 **encadré** `mcp-action`
> Au-delà du contexte : agir
> Un MCP ne sert pas seulement à obtenir de l'information — il peut aussi déclencher
> des actions : envoyer un email, modifier un fichier, créer une entrée dans un ERP.
> Le modèle ne se contente plus de répondre, il fait.
> C'est le premier pas vers les agents — assistants autonomes capables d'enchaîner
> des actions sans intervention humaine à chaque étape. Renvoi ch. XX.

#### Contenu

**Pourquoi le contexte est nécessaire**
S'appuyer sur l'analogie du collaborateur introduite au ch01. Un collaborateur qui arrive
sans briefing produit du travail générique — compétent mais à côté. Le contexte c'est
ce qu'on lui donne avant de commencer :
- **Contenu** : les objectifs du projet, votre situation, vos documents, le persona
  assigné au modèle (expert juridique, coach bienveillant, relecteur critique...)
- **Contenant** : la culture d'entreprise — style, ton, format, contraintes de comportement
  (ex : "pose des questions avant de répondre" contrecarre la tendance naturelle du LLM
  à agir avant de réfléchir)

Illustrer avec l'exemple de la réunion de gouvernance : sans contexte, ordre du jour
générique. Avec contenu (membres du bureau, décisions en suspens, compte-rendu précédent)
+ contenant (synthèse en cinq points, ton factuel, une proposition de décision par point) :
document utilisable en dix minutes.

Poser en fin de section l'accroche vers ch06 : même un collaborateur bien briefé peut
se perdre si on lui donne trop d'informations en vrac. Le contexte joue un second rôle
dans le mécanisme d'attention — on y reviendra au chapitre suivant.

**La limite fondamentale : du texte plat**
Le LLM reçoit un grand texte à compléter — et c'est tout. Il ne peut pas hiérarchiser
mécaniquement les éléments du contexte : on lui dit ce qui est important, et on espère
qu'il comprend. Utiliser figure prompt-etendu-complet ici.

**Le contexte statique : ce que l'opérateur vous offre**
- Instructions utilisateur : insérées automatiquement dans chaque prompt étendu
- Projet : contexte partagé par un groupe de sessions (instructions, documents, mémoire)
- Fichiers de référence : injectés dans le prompt — utiles mais coûteux en tokens,
  à doser (le contexte qui grossit trop dilue le focus — on verra pourquoi au ch06)

**La mémoire**
Mécanisme de base : le modèle ne se souvient de rien entre les sessions. La mémoire
est du texte injecté dans le prompt système, clairement balisé. Le modèle sait qu'elle
est là. Il peut proposer des mises à jour si la session apporte des éléments pertinents.
Transparent et contrôlable — c'est le mécanisme honnête.

Voir encadré `memoire` pour ce que les opérateurs en font en pratique.

**Le contexte dynamique**
Jusqu'ici, le contexte est statique : défini à l'avance, injecté mécaniquement.
Autre approche : laisser le modèle aller chercher lui-même le contexte qui lui manque.
Au lieu d'un seul tour de génération, on en fait plusieurs — une génération peut formuler
une demande de contexte supplémentaire, que l'opérateur va chercher avant de relancer.

Exemple narratif : "Fait-il beau à Nice ?" — le modèle ne sait pas, mais il sait
qu'il peut faire une recherche web. Il la demande, l'opérateur l'exécute, le modèle
reçoit le résultat et répond.

**Skills et MCP : le contexte dynamique généralisé**
La recherche web n'est qu'un exemple. Les outils que le modèle peut solliciter
constituent un écosystème :
- **Skill** : ensemble d'instructions décrivant une compétence à activer à la volée
- **MCP** (Model Context Protocol, défini par Anthropic) : description d'un service
  externe — liste de commandes disponibles + instructions d'usage en langage naturel

L'écosystème MCP est vaste : emails, fichiers, cloud, bases de données, ERP, modèles 3D...
Voir encadré `codage` pour une illustration concrète des deux dimensions contenu/contenant.
