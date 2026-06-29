## 2. Le modèle et l'opérateur

Votre chatbot favori n'est pas un seul composant. Sous le capot, il y en a au moins
deux: un {{def:mk:modèle_de_langage}} et un {{def:mk:opérateur}}, et ils ne tournent
pas forcément sur les mêmes serveurs, ni chez le même fournisseur.

{{def:fig:flux-opérateur}}

La figure dit tout, ou presque. Vous posez une question : l'opérateur la reçoit,
la prépare, l'envoie au modèle. Le modèle produit une réponse — on appelle ça une
{{def:mk:complétion}}. L'opérateur la récupère et vous l'affiche. Aller-retour.
Vous ne parlez jamais directement au modèle.

### Le modèle de langage

Le {{ref:mk:modèle_de_langage}}, souvent désigné par l'acronyme {{def:mk:LLM}}
(*Large Language Model*), fait une chose : compléter un texte inachevé.

Par exemple la phrase *"La capitale de la France est"* reste suspendue. Sa complétion la plus
probable est : *"Paris"*. Le consensus est fort, c'est ce que dit la grande majorité
des milliards de textes sur lesquels le modèle a été entraîné. Vichy reste une
réponse techniquement plausible, mais très minoritaire. On verra au chapitre suivant
comment fonctionne cet entraînement.

*"La meilleure équipe de foot en France est"*, c'est plus intéressant. Plusieurs
complétions sont plausibles, le consensus moins net. Quelque chose va sortir, mais
ce sera polémique.

Reformulons : *"De l'avis des principaux commentateurs sportifs, les trois meilleures
équipes de foot en France sont"*. La complétion devient plus riche, plus prévisible.
La façon de formuler oriente le résultat. C'est déjà une intuition sur le rôle du
{{def:mk:prompt}}, on y reviendra.

Ce mécanisme fondamental de complétion de phrase s'appele la {{def:mk:génération}}, d'où le nom d'IA générative.
### L'opérateur

L'{{def:mk:opérateur}}, c'est tout le reste. L'interface, la mise en forme, la
gestion de la conversation. Il reçoit votre {{ref:mk:prompt}}, le prépare pour le
modèle, récupère la complétion, et vous l'affiche sous une forme lisible.

On l'appelle aussi orchestrateur, surtout quand il gère plusieurs modèles ou
coordonne des tâches complexes, ou harnais (*harness* en anglais), quand son rôle
se limite à relayer le prompt sans beaucoup de traitement. Même animal, noms
différents selon les contextes. Dans ce guide on dira opérateur.

Ce que l'opérateur fait surtout, c'est glisser devant votre question un ensemble
d'instructions que vous ne voyez pas. C'est ce qu'on appelle le {{def:mk:prompt_système}}.

{{def:enc:prompt-systeme}}
> ### Le prompt système
> *L'IA vous explique*
>
> Avant votre question, l'opérateur en glisse une autre, plus longue, invisible,
> soigneusement rédigée. C'est le prompt système. Il ressemble à quelque chose comme :
>
> *« Tu es un assistant IA utile et précis. Tu réponds de façon claire et concise.
> Si une question est ambiguë, tu demandes une clarification. Tu ne donnes pas
> d'informations non vérifiées. Tu restes dans le sujet demandé. [...] »*
>
> C'est lui qui définit le ton, le format, le comportement face aux ambiguïtés, les
> sujets autorisés ou interdits, la relance de la conversation. Deux assistants utilisant exactement le même modèle peuvent répondre très différemment à la même question, parce que leurs prompts
> systèmes sont différents. C'est un secret de fabrication : complexe, finement
> réglé, propre à chaque opérateur, jamais documenté.
>
> Votre propre formulation joue le même rôle côté utilisateur. Pas besoin de magie :
> être clair et donner du contexte suffit dans la grande majorité des cas.

C'est l'opérateur qui façonne l'expérience que vous avez de l'IA. Le modèle est
le moteur. L'opérateur, c'est la carrosserie, le tableau de bord, et le GPS.

### Infrastructure et économie

Le modèle tourne sur des serveurs très spécialisés — pas le genre de machine qu'on
trouve dans une salle informatique ordinaire.

{{def:enc:GPU}}
> ### Serveurs et GPU — pourquoi ça coûte cher
> *L'IA vous explique*
>
> Un grand modèle de langage ne tourne pas sur un ordinateur portable. Il mobilise
> des centaines, parfois des milliers de processeurs graphiques — les GPU — conçus
> pour effectuer des millions de calculs en parallèle. Ce sont les mêmes puces qui
> font tourner les jeux vidéo en haute définition, mais ici on en empile des hangars
> entiers, 24h/24.
>
> Ce n'est pas un logiciel qu'on installe : c'est une usine qu'on loue à la requête.
> Chaque fois que vous envoyez un message, quelque part dans le monde, des milliers
> de processeurs s'activent pendant une fraction de seconde.
>
> Côté économique, le modèle facture à l'opérateur chaque appel suivant sa complexité, en gros la longueur du message à compléter en entrée et la longueur du message complété en sortie, c'est ce qu'on
> appelle la facturation API, notez que l'on paye pour le prompt système. L'opérateur vous facture ensuite en SaaS : abonnement
> mensuel, version gratuite limitée, version premium. Le modèle freemium que vous
> connaissez.
>
> C'est aussi pourquoi la course aux investissements est aussi intense : construire
> et faire tourner ces infrastructures coûte des milliards. Les grands acteurs se
> disputent les GPU disponibles dans le monde entier.

Ce double modèle économique, API côté opérateur, SaaS côté utilisateur, explique
une partie du paysage commercial. Les fournisseurs qui intègrent les deux (modèle
et opérateur sous le même toit) sont naturellement moins transparents sur leur
refacturation interne. Ce qui alimente quelques rumeurs sur les réseaux. C'est leur
droit.

### Le paysage des opérateurs

Maintenant qu'on a posé les bases, voilà où ça mène concrètement.

Les opérateurs sont partout, sous des formes très différentes. Voici comment s'y
retrouver.

{{def:tab:operateurs}}

| Catégorie                      | Exemples                                           | Usage principal                         |
| ------------------------------ | -------------------------------------------------- | --------------------------------------- |
| Chatbots généralistes          | ChatGPT, Claude, Gemini, LeChat                    | Conversation libre, rédaction, analyse  |
| Assistants intégrés            | Copilot (Microsoft 365), Gemini (Google Workspace) | Assistance dans les outils du quotidien |
| Assistants de codage           | Cursor, GitHub Copilot, Claude Code                | Écriture et révision de code            |
| Moteurs de recherche augmentés | Perplexity, SearchGPT                              | Recherche avec synthèse et sources      |
| Agents                         | —                                                  | Voir ci-dessous                         |

Les agents méritent une mention à part. Ce sont des opérateurs qui ne se contentent
pas de relayer une question, ils décomposent une tâche complexe, appellent plusieurs
modèles ou outils en séquence, et gèrent des boucles de décision. Un {{def:mk:agent}}
peut par exemple chercher des informations en ligne, les synthétiser, rédiger un
document, et l'envoyer par email, sans intervention humaine à chaque étape.

Ce qui importe ici : tous ces opérateurs, aussi différents soient-ils en apparence,
reposent sur le même mécanisme. La diversité est en surface.

### Les modèles sous le capot

Derrière cette diversité d'opérateurs, combien de modèles ?

Moins qu'on ne le croit.

{{def:tab:LLM}}

| Famille | Éditeur | Modèle | Utilisé par |
|---|---|---|---|
| GPT | OpenAI | Propriétaire | ChatGPT, Copilot, des centaines d'autres |
| Claude | Anthropic | Propriétaire | Claude.ai, intégrations diverses |
| Gemini | Google | Propriétaire | Gemini, Google Workspace |
| Llama | Meta | {{def:mk:open_source}} | Des dizaines d'opérateurs, dont Perplexity |
| Mistral | Mistral AI | Open source (en partie) | Le Chat, intégrations européennes |

Cette liste n'est pas exhaustive, loin de là. Les modèles chinois, DeepSeek et Qwen
en tête, ont fait une entrée remarquée et talonnent les leaders occidentaux. Le paysage
bouge vite.

La colonne "utilisé par" est instructive. GPT-4o alimente ChatGPT, mais aussi
Copilot, et des centaines d'opérateurs tiers qui appellent l'API d'OpenAI. Llama,
parce qu'il est open source, se retrouve partout.

La plupart des grands fournisseurs proposent plusieurs versions du même modèle,
légère et rapide pour les tâches courantes, puissante et coûteuse pour les cas
complexes par exemple les poétiques: Haiku, Sonnet, Opus et Fable d'Anthropic. C'est la différenciation par puissance : on y reviendra au chapitre suivant.

{{def:enc:open-source}}
> ### Open source et commoditisation des modèles
> *L'IA vous explique*
>
> Un modèle open source, c'est un modèle dont les paramètres sont publiquement
> disponibles. N'importe qui peut le télécharger, le modifier, l'héberger sur ses
> propres serveurs, le faire tourner sans payer de licence.
>
> Meta a fait ce choix avec Llama — et c'est un choix stratégique autant que
> philosophique. En publiant son modèle, Meta a déclenché une prolifération
> d'opérateurs qui n'auraient pas pu se payer l'accès aux modèles propriétaires.
> Et une pression sur les prix de l'ensemble du marché.
>
> La thèse qui se dessine : les modèles tendent à se banaliser. La vraie valeur
> se déplace vers les opérateurs, leur interface, leur prompt système, leur
> intégration dans les workflows. Comme les moteurs à explosion sont devenus une
> commodité : ce qui différencie les voitures, ce n'est plus le moteur. Méfiez-vous aussi des opérateurs trop simples, ce sont des go-karts.
>
> Vers les opérateurs, oui, mais aussi vers ceux qui fabriquent les pioches. Dans
> toute ruée vers l'or, c'est le marchand de jeans qui s'enrichit le plus sûrement.
> Nvidia en sait quelque chose.
>
> Pour le lecteur, le message pratique est simple : le modèle sous le capot compte
> moins que ce qu'on en fait.

Ce mouvement de commoditisation est loin d'être terminé. Mais il explique déjà
pourquoi changer de modèle dans votre chatbot favori n'est pas toujours aussi
décisif qu'on pourrait le croire, et pourquoi des opérateurs sans modèle propre
peuvent très bien tirer leur épingle du jeu.  On verra qu'il y a aussi une 'tendance' à la surconsommation qui peut être maîtrisée avec un peu d'expérience.
