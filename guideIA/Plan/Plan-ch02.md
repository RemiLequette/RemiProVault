# Plan — Chapitre 2

### 2. Le modèle et l'opérateur

#### Mots-clés

- modèle de langage
- LLM
- opérateur
- prompt
- complétion
- prompt système

#### Figures

> 🖼️ **figure** `flux-opérateur`
> Diagramme du flux entre les deux composants :
> humain → prompt → opérateur → prompt étendu → modèle → complétion → opérateur → humain.
> Flux aller (prompt) et retour (réponse) distincts entre chaque composant.
> Trois nœuds : humain (gris, neutre), opérateur (teal), modèle/LLM (violet).
> Style épuré, flèches directionnelles, labels courts.
> Enrichir avec les données externes sur l'opérateur (web...) — philosophie "complet mais juste assez" :
> montrer que l'opérateur peut accéder à des données externes sans les détailler dans le texte.
> Le lecteur voit l'architecture complète dès le ch. 3 et la visite progressivement dans les chapitres suivants.

#### Encadrés

#### Contenu

**Les deux composants**
La figure présente l'architecture de base d'un système IA utilisant les techniques générative de language. Elle repose sur deux process en général sur des serveurs différents: le modèle de langage et l'opérateur. L'opérateur présente une interface à l'utilisateur, il recoit une question / requête (le "prompt"). Il "l'enrobe" dans un message d'entrée qui est transmis au modèle, celui ci le "complète" (on va voir ce que ca signifie) pour obtenir un message de sortie. L'opérateur recoit ce message et en extrait une réponse qu'il présente à l'utilisateur. Et la boucle est bouclée on repart pour un tour de manège.

Figure

**Le modèle de langage (LLM) et la génération**
Un modèle de langage, pour pouvoir être utilisé, a du passer par une longue phase d'entrainement ou on lui a présenté des milliards de textes (pour les modèles 'multi-modaux' il y a aussi des images et du son, mais ca ne change pas profondément le mécanisme, il y a plus de détails en annexe).

Après cete apprentissage le modèle a "ingéré" une énorme quantités d'information qu'il a "conceptualisé", le terme est vague, mais celà veut dire qu'il peut passer d'un mot comme Paris à un concept qui entretient des rapports de proximité mesurables avec beaucoup d'autre concepts: France, capitale, ville, Seine, Lutèce, Tour Eiffel, Ville Lumière,Louvre, Joconde, Hausmannien, Jaques Chirac, Montmartre, Commune de Paris, Ernest Hemingway.... 

Armé de ces connaissances on s'attend du moteur qu'il se livre sur le message d'entrée a une activités assez curieuse, une completion sémantique, on l'appelera 'génération', on parle aussi d'inférence, mais je préfère garder ce mot pour les annexes.

Le principe est de considérer le message d'entrée comme un texte 'incomplet' et de le completer pour que le message de retour soit le texte qui prolonge le message d' entrée de la façon la plus probable.

Vite! un exemple:  supposons que le message d'entrée soit "La capitale de la France est". Cette phrase reste suspendue, elle a "tension cognitive" sa complétion la plus
probable est "Paris". Le consensus est fort. Vichy reste une réponse plausible, mais
minoritaire, le modèle reflète le consensus de ses données d'entraînement, l'ajout d'une date aurait tué toutes hésitations.

"La meilleure équipe de foot en France est" — c'est plus intéressant. Plusieurs
complétions sont plausibles, le consensus est moins évident. Quelque chose va sortir, mais ce sera plus polémique...

Et si on reformule : "De l'avis des principaux commentateurs sportifs, les trois
meilleures équipes de foot en France sont" — la complétion devient plus prédictibe et plus riche, le message d'entrée était plus précis. On a déjà une intuition de la bonne manière de rédiger les prompts.

Le mot "prompt" n'est pas anodin: il montre bien qu'il s'agit d'une "amorce" de texte invitant la génération.

**L'opérateur**
C'est le composant qui fait l'interface avec l'humain. Il reçoit le prompt, l'envoie au modèle, récupère la complétion et l'affiche. Le nom 'opérateur' est moins consensuel que 'modèle', certains l'appelent 'opérateur', mais surtout quand il gère plusieurs modèles, ou 'harnais' mais surtout quand il joue un rôle très simple avec peu de traitement du prompt, et on verra qu'il y en a beaucoup dans les opérateurs que vous connaissez.

**Quelques exemples**

Table des exemples

**Retour sur l'architecture**

Cette architecture est plaisante pour un ingénieur informatique car elle est simple et sépare bien les responsabilités et les contraintes techniques.

- Le modèle a besoin d'une forte capacité de calcul (encadré les GPU) alors que l'opérateur est une application WEB bien dans la moyenne (bien moins complexe qu'un site de e-commerce par exemple)
- Le modèle "sert" de nombreux opérateurs en paralèle de nombreuses instances du même ou des variantes suivant les usages
- Les modèles sont interchangeables suivant la complexité de la tache à accomplir (vous avez du tous remarqué dans vos chatbots que vous pouviez changer de modèle)
- La communication repose sur des standards informatiques que l'on appelle souvent API (Application Programing Interface)
- Modèles et opérateurs peuvent être développés par des entités différentes, commerciales ou open-source. Autant il y a peu de fournisseurs de modèles qui proposent en général leur propre opérateur façon chatbot. Autant il y a beaucoup d'opérateurs qui fournissent des expériences utilisateur différentes, par exemple les assistant de codage integrés dans les environnements de développement, ou des gestionnaires de workflows, ou tout autre façon d'enrichir un processus avec de la communication en language naturel.

A celà s'ajoute un double modèle commercial, le service du modèle est facturé à l'opérateur (c'est un cout d'usage de l'API, avec un cout proportionnel à la taille des messages entrée-sortie, allez on vous dit tout de suite que ca se mesure en 'tokens', on en reparlera, mais en gros c'est des mots) et l'opérateur facture à l'utilisateur avec des modèles SaaS classiques freemium.

Naturellement les fournisseurs "verticalement intégrés" qui offrent les deux sont des plus discrets sur la refacturation interne, si tenté qu'elle existe. Evidemment toute opacité créant des théories complotistes, la rumeur court que le cout de la génération est bien moindre via la médiation de l'opérateur maison que via les API.

Je soupconne cependant une autre raison aux plaintes sur les réseaux sociaux qui semblent bien exagérer. Si vous voulez vous vanter d'être un développeur IA, faire des modèles est absolument hors de portée, la barrière est très élevée. Par contre bricoler des opérateurs est à la portée de beaucoup plus. Méfions nous cependant, la contribution de l'opérateur est souvent sous-estimée, et surtout le couplage entre le modèle et l'opérateur devient de plus en plus fort et il n'est pas toujours documenté.

**Le problème de l'interface**
Personne ne veut parler directement au modèle — en lui soumettant des débuts de phrase à compléter. C'est là que l'opérateur prend la main : il rajoute devant la question — sans la comprendre — des instructions pour que la réponse soit utile et cohérente. C'est ce qu'on appelle le prompt système.

[bloc de code exemple prompt système]

Avec ce prompt système, on peut espérer une complétion du style :

> La capitale de la France est Paris, sur la Seine. C'est aussi la plus grande ville du
> pays avec 2 millions d'habitants. Y a-t-il d'autres pays dont vous voudriez connaître
> la capitale ?

Cet exemple est purement imaginaire — il veut simplement mettre en évidence l'importance
du prompt système pour la qualité de la réponse : ton, format, gestion des relances,
comportement en cas d'ambiguïté. C'est un secret de fabrication : complexe, finement
réglé, propre à chaque IA. Tout n'est pas dans le modèle. (voir encadré, annexe)

La rédaction de votre propre prompt est aussi importante. Mais si on comprend le
principe, c'est d'abord du bon sens : être clair et fournir du contexte pour bien encadrer la génération.
