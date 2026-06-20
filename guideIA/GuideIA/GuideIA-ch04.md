## 4. C'est quoi la génération ?

Il y a quelque chose de presque magique dans ce que fait un LLM. On comprend que ses connaissances viennent de textes humains — mais nulle part dans la littérature humaine il n'existe un texte aussi spécifique que le verbiage d'un prompt système sur mesure. Comment le modèle peut-il produire quelque chose d'utile à partir de ça ?

La réponse tient en un mot : interpolation. Pour l'expliquer, partons d'un exemple volontairement simple.

### Un modèle immobilier

Je veux estimer la valeur d'un appartement. Je dispose d'une liste de transactions immobilières avec les surfaces et les prix.

Approche classique : calculer le prix moyen au m², puis multiplier par la surface de mon appartement.

{{def:fig:immo-regression}}

Sans le savoir, je viens de construire un modèle. Un modèle, c'est trois choses :

- une **procédure de calcul** : multiplier par le prix au m²
- des **paramètres** qui règlent ce calcul : ici un seul, le prix moyen au m²
- une **méthode d'apprentissage** pour trouver les paramètres à partir de données connues — ici les transactions immobilières, et la méthode le calcul de la moyenne

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

**La qualité des données.** *Garbage in, garbage out.* Un exemple typique : des valeurs aberrantes — un bien bradé, un vendu très au-dessus du marché — qui faussent le calcul des paramètres.

{{def:fig:immo-outliers}}

Pour les LLM, c'est le même principe : gros travail de préparation des données, filtrage, nettoyage. Wikipedia, c'est bien. Les forums complotistes, beaucoup moins.

**La puissance du modèle.** Il y a peut-être plus que juste la surface : quartier, âge de l'immeuble, étage... Et même avec une seule variable, la puissance du modèle joue. Les petites surfaces sont souvent plus chères au m² — notre droite ne capture pas ça. On peut enrichir le modèle : deux pentes, une pour les petites surfaces, une pour les grandes.

{{def:fig:immo-deux-segments}}

Quand un modèle n'est pas assez puissant pour produire des résultats satisfaisants, on dit qu'il fait de l'{{def:mk:underfitting}}. Vous aurez beau l'entraîner davantage, il ne progressera pas — il lui manque la structure pour répondre à la question.

À l'inverse, un modèle très complexe avec trop de paramètres par rapport aux données disponibles peut "apprendre par cœur" les exemples sans généraliser. Sa courbe passe par tous les points connus, mais part dans des directions improbables entre eux. C'est l'{{def:mk:overfitting}}.

{{def:fig:immo-underfitting}}

{{def:fig:immo-overfitting}}

Techniquement, on parle d'{{def:mk:interpolation}} quand les questions tombent *entre* les données d'entraînement, et d'{{def:mk:extrapolation}} quand elles tombent *en dehors*. Dans les deux cas, le modèle fait de son mieux — mais extrapoler l'amène encore plus facilement dans la science-fiction.

{{def:fig:immo-interpolation-extrapolation}}
