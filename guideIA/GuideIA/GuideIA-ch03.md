## 3. C'est quoi la génération ?

Il y a quelque chose de presque magique dans ce que fait un LLM. On comprend que ses connaissances viennent de textes humains — mais nulle part dans la littérature humaine il n'existe un texte aussi spécifique que le verbiage d'un prompt système sur mesure. Comment le modèle peut-il produire quelque chose d'utile à partir de ça ?

La réponse tient en un mot : interpolation, une technique mathématique, plus précisément statistique dont les origines remontent au début du XIXème siècle avec Gauss et que l'on appelle aussi régression. Pour l'expliquer, partons d'un exemple volontairement simple.

### Un modèle immobilier

Je veux estimer la valeur d'un appartement. Je dispose d'une liste de transactions immobilières avec les surfaces et les prix.

Approche classique : calculer le prix moyen au m², puis multiplier par la surface de mon appartement.

{{def:fig:immo-regression}}

Sans le savoir, je viens de construire un modèle. Un modèle, c'est trois choses :

- une **procédure de calcul** : multiplier par le prix au m²
- des **paramètres** qui règlent ce calcul : ici un seul, le prix moyen au m²
- une **méthode d'apprentissage** pour trouver les paramètres à partir de données connues, ici les transactions immobilières, et la méthode le calcul de la moyenne

Même si la surface de mon appartement ne figure dans aucune transaction historique, ou si plusieurs transactions ont la même surface avec des prix différents, j'obtiens une estimation raisonnable. C'est ça, la magie de l'interpolation : généraliser à partir d'exemples, sans avoir besoin d'un exemple pour chaque cas.

### Le LLM : même concept, autre échelle

Un LLM fonctionne sur le même principe, à une échelle radicalement différente :

- **Entrées/sorties** : des débuts de texte et leur complétion
- **Données d'{{def:mk:données_dentraînement}}** : une immense base de textes, chacun découpé en couples (début, complétion)
- **Procédure de calcul** : c'est la {{def:mk:génération}} — complexe, mais rapide à exécuter une fois le modèle construit (les fameux GPU y contribuent)
- **Paramètres** : plusieurs milliards, parfois dizaines de milliards
- **Apprentissage** : extrêmement coûteux en calcul, qui ne se refait pas tous les jours

### D'où viennent les hallucinations

Notre modèle immobilier peut donner des évaluations irréalistes. Pourquoi ? Deux raisons : la qualité des données et la puissance du modèle.

**La qualité des données.** Un exemple typique : des valeurs aberrantes — un bien bradé, un vendu très au-dessus du marché — qui faussent le calcul des paramètres.

{{def:fig:immo-outliers}}

Vous diriez que ce sont de vraies transactions, certes, mais reflètent elles vraiment la réalité du marché?

Pour les LLM, c'est le même principe : gros travail de préparation des données, filtrage, nettoyage. Wikipedia, c'est bien. Les forums complotistes, beaucoup moins.

**La puissance du modèle.** Il y a peut-être plus que juste la surface : quartier, âge de l'immeuble, étage... Et même avec une seule variable, la puissance du modèle joue. Les petites surfaces sont souvent plus chères au m² — notre droite ne capture pas ça. On peut enrichir le modèle : deux pentes, une pour les petites surfaces, une pour les grandes.

{{def:fig:immo-deux-segments}}

Quand un modèle n'est pas assez puissant pour produire des résultats satisfaisants, on dit qu'il fait de l'{{def:mk:underfitting}}. Vous aurez beau l'entraîner davantage, il ne progressera pas, il lui manque la structure cérébrale pour répondre à la question.

À l'inverse, un modèle très complexe avec trop de paramètres par rapport aux données disponibles peut "apprendre par cœur" les exemples sans généraliser. Sa courbe passe par tous les points connus, mais part dans des directions improbables entre eux. C'est l'{{def:mk:overfitting}}.

{{def:fig:immo-underfitting}}

{{def:fig:immo-overfitting}}

Techniquement, on parle d'{{def:mk:interpolation}} quand les questions tombent *entre* les données d'entraînement, et d'{{def:mk:extrapolation}} quand elles tombent *en dehors*. Dans les deux cas, le modèle fait de son mieux — mais extrapoler l'amène encore plus facilement dans la science-fiction.

{{def:fig:immo-interpolation-extrapolation}}

La composition des données d'entraînement introduit d'autres biais, plus subtils. La littérature disponible sur internet est majoritairement en anglais — ce qui signifie que le modèle connaît mieux certaines cultures, certaines références, certaines façons de raisonner. Et les données ne sont pas toutes de même qualité : un article de fond n'a pas le même poids qu'un post de forum. Les équipes qui construisent les modèles travaillent à filtrer, pondérer, corriger — mais aucun corpus n'est neutre.

Il y a une raison supplémentaire, plus profonde : les textes écrits ne disent presque jamais "je ne sais pas". C'est quelque chose qu'on apprend à l'oral, dans l'échange, dans le doute partagé. On n'écrit pas pour consigner ses incertitudes — on écrit pour transmettre ce qu'on croit savoir. Le modèle a donc ingéré une littérature massivement assertive, et il en a pris les habitudes.

À cela s'ajoute une couche d'ajustement après entraînement : des humains évaluent les réponses du modèle et leurs retours orientent un second apprentissage. C'est une façon efficace de rendre le modèle plus utile et plus sûr, mais c'est aussi une nouvelle source de biais, liés cette fois aux préférences et aux angles morts de ceux qui évaluent. Nous reviendrons sur ce mécanisme dans l'annexe sur l'apprentissage ({{ref:ch:ch15}}).

{{def:enc:biais-de-positivite}}

### Ce que l'interpolation ne fait pas

Jusqu'ici, on a parlé de la qualité des réponses. Mais il y a une question plus fondamentale : est-ce que le modèle *raisonne* ?

La réponse courte est non, et c'est important à comprendre.

La génération se fait en une étape. Le modèle reçoit un texte, calcule la complétion la plus probable, et s'arrête là. Quand on lui demande de "justifier sa réponse", il ne revient pas en arrière vérifier ce qu'il vient de dire. Il interpole des *schémas de justification*, il reproduit la forme d'un raisonnement, telle qu'elle apparaît dans ses données d'entraînement, sans en faire le fond.

Demandez à un LLM où est la Joconde : Paris, immédiatement, c'est dans tous les guides. Demandez-lui où sont les Noces de Cana : il connaît ce grand tableau de Véronèse, il sait qu'il est au Louvre, il sait que le Louvre est à Paris — et pourtant, le petit syllogisme qui vous vient naturellement en une fraction de seconde peut lui résister. Ce n'est pas de l'ignorance : c'est la limite structurelle de la génération. Elle complète, elle n'enchaîne pas. On verra dans les chapitres suivants comment contourner ça — c'est tout un art.

Ce serait injuste de s'arrêter là. Le même modèle connaît la date de chaque bataille napoléonienne, les propriétés de chaque molécule, le texte de milliers de jugements. L'étendue est vertigineuse. Mais cette limite-là est structurelle — elle ne disparaîtra pas avec un modèle plus puissant. C'est le prix à payer pour une machine qui génère, plutôt que pour une machine qui raisonne.

Ce n'est pas de la mauvaise volonté. C'est la nature de la génération: elle produit ce qui *ressemble* à une bonne réponse, pas ce qui *est* une bonne réponse. On va se permettre une autre comparaison car c'est très important de comprendre ce phénomène.

Ceux des lecteurs qui ont fait du latin se rappellent peut-être du "Gaffiot", c'est un dictionnaire extrêmement complet et surtout qui contient beaucoup de citations complètes et traduites. Avec un peu de chance, et d'expérience, on peut tomber sur le mot qui va nous donner toute la phrase de la version à traduire. Est-on devenu un bon latiniste? Non, quelque part on a triché. Je laisse de coté les grandes discussion sur IA et éducation, en effet, faire ses devoirs avec l'IA c'est comme s'acheter un vélo d'appartement électrique. Mais il y a un autre effet "Gaffiot", l'IA elle-même vous répond juste car elle a trouvé dans le dictionnaire, pas parce qu'elle est intelligente.

Et malheureusement, comme le mauvais latiniste très imaginatif, lorsqu'il ne sait pas, il va inventer une histoire: *"Si non è vero, è ben trovato"* comme on dit si joliment en Italie.

### Une IA n'a que des croyances

C'est le fil conducteur de tout ce qui suit.

Un LLM ne sait pas. Il *croit* — avec plus ou moins de confiance, selon la densité de ses données d'entraînement sur le sujet. Et il n'a aucune notion d'avoir tort, parce que cette notion suppose un retour sur soi, une vérification, un doute actif. Ce sont des opérations de {{ref:mk:raisonnement}}, pas d'interpolation.

Cela ne veut pas dire que l'IA est inutile. Cela veut dire qu'elle a besoin de vous pour jouer le rôle que le {{ref:mk:systeme2}} joue dans votre propre pensée : vérifier, douter, corriger. C'est ce que nous verrons dans les chapitres suivants.
