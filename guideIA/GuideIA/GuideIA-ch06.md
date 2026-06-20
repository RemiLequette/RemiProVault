## 7. Le problème de l'attention

Ce chapitre revient sur le mécanisme qui a tout changé — et explique pourquoi les contextes longs peuvent nuire à la qualité des réponses.

### L'insight fondateur

Le langage est ambigu par nature. Ce n'est pas un défaut — c'est sa caractéristique fondamentale. Le sens d'un mot dépend des mots qui l'entourent.

{{def:enc:polysemie}}

Prenez le mot *tour*. Tour Eiffel, Tour de France, tour de potier, pièce aux échecs, tour de chant, tour de vis, ordre dans une file, donjon médiéval... Et *"il grimpe dans les tours"* : régime moteur ou quelqu'un qui s'énerve ?

C'est précisément ce défi — résoudre l'ambiguïté du langage — qui a conduit à la percée technologique des LLM : l'**{{def:mk:attention}}**. Chaque mot "regarde" tous les autres et calcule à quel point ils sont pertinents pour lui. Ce mécanisme permet de pondérer l'ensemble du contexte simultanément, et non mot à mot.

### Ce que ça coûte

Ce mécanisme est puissant mais exigeant : son coût de calcul croît très vite avec la longueur du contexte. Plus le contexte est long, plus le calcul est lourd — et plus la qualité peut se dégrader.

### Le problème de la dilution

Un contexte long ne signifie pas une meilleure compréhension. L'{{ref:mk:attention}} doit se répartir sur l'ensemble du texte. Si ce texte contient beaucoup d'éléments peu pertinents, la concentration se dilue sur des informations inutiles.

{{def:fig:attention-dilution}}

C'est contre-intuitif : on a l'impression qu'en donnant plus d'informations, on aide. En réalité, on peut nuire.

Analogie : un jury qui doit trancher un litige. Lui soumettre 10 documents clairs est plus efficace que 200 pièces dont 190 sont hors sujet. La pertinence prime sur le volume.

### Le "lost in the middle"

Dans un contexte très long, les informations placées au milieu sont statistiquement moins bien traitées que celles en début ou en fin de contexte. C'est ce qu'on appelle le phénomène de {{def:mk:lost_in_the_middle}}.

Implication pratique : si vous avez une information critique, ne la noyez pas au milieu d'un long document.

### La {{ref:mk:fenêtre_de_contexte}} et la {{def:mk:dilution}}

Ces deux contraintes — taille maximale et dégradation par dilution — convergent vers les mêmes conseils pratiques :

- Des prompts courts et ciblés
- Les informations importantes en tête ou en queue de contexte
- Ne pas injecter des documents entiers si seules quelques sections sont pertinentes
- Reconnaître les signes de dégradation : réponses qui ignorent des instructions données, incohérences avec des éléments mentionnés plus tôt dans la session

### La {{def:mk:self-attention}} et la {{def:mk:polysémie}}

Le mécanisme d'attention résout la polysémie — mais il le fait dans les deux sens. Tout comme le contexte aide à désambiguïser un mot, un contexte trop chargé ou mal structuré peut introduire de nouvelles ambiguïtés. Un document qui traite de plusieurs sujets à la fois peut amener le modèle à mélanger des fils pourtant distincts.

C'est une raison supplémentaire pour la règle d'or : une session, un sujet.
