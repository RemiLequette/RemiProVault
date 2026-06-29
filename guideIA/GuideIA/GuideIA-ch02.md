## 2. Le modèle et l'opérateur

Un assistant IA n'est pas un seul composant monolithique. Sous le capot, il y en a au moins deux — et comprendre leur rôle respectif change la façon dont on l'utilise.

### Le modèle de langage

Le premier composant est le {{def:mk:modèle_de_langage}} — souvent désigné par l'acronyme {{def:mk:LLM}} (*Large Language Model*). Son rôle est précis : compléter un texte inachevé.

Prenons un exemple simple. La phrase *"La capitale de la France est"* reste suspendue. Sa complétion la plus probable est *"Paris"*. Le consensus est fort — c'est ce que dit la grande majorité des textes sur lesquels le modèle a été entraîné. Vichy reste une réponse techniquement plausible, mais très minoritaire.

Maintenant : *"La meilleure équipe de foot en France est"*. C'est plus intéressant. Plusieurs complétions sont plausibles, le consensus moins net.

Reformulons encore : *"De l'avis des principaux commentateurs sportifs, les trois meilleures équipes de foot en France sont"*. La {{def:mk:complétion}} devient plus riche, plus nuancée. C'est déjà une introduction au rôle du {{ref:mk:prompt}} : la façon de formuler oriente le résultat.

### L'opérateur

Le second composant est l'{{def:mk:opérateur}}. C'est lui qui fait l'interface avec vous. Il reçoit votre message, le prépare pour le modèle, envoie le tout, récupère la réponse, et vous l'affiche.

{{def:fig:flux-opérateur}}

### Le secret de fabrication

Personne ne veut parler directement au modèle brut — en lui soumettant des débuts de phrase à compléter. C'est là qu'intervient l'opérateur : avant votre question, il glisse un ensemble d'instructions que vous ne voyez pas. C'est ce qu'on appelle le {{def:mk:prompt_système}}.

Ça ressemble à quelque chose comme :

```
Tu es un assistant IA qui répond à une question posée par un utilisateur. Tu dois donner
des informations claires et précises, avec une explication courte, voire plusieurs
possibilités s'il y a des ambiguïtés. Tu poses des questions de clarification si
nécessaire. Tu peux ajouter quelques informations complémentaires qui restent dans le
thème, et tu relances la conversation avec une question ouverte pour engager des demandes
d'informations complémentaires. [...]
```

Avec ce prompt système, la complétion de *"La capitale de la France est"* pourrait donner :

> *La capitale de la France est Paris, sur la Seine. C'est aussi la plus grande ville du pays avec 2 millions d'habitants. Y a-t-il d'autres pays dont vous voudriez connaître la capitale ?*

Cet exemple est imaginaire — il illustre simplement l'importance du prompt système : ton, format, gestion des relances, comportement face aux ambiguïtés. C'est un secret de fabrication complexe, finement réglé, propre à chaque assistant.

Votre propre formulation compte autant. Mais si on comprend le principe, c'est d'abord du bon sens : être clair, fournir du contexte. N'écoutez pas trop les pseudo-gourous qui vendent des prompts soi-disant tunés — *"Tu es un professeur de danse spécialisé dans les claquettes..."* Ce genre de formule peut même être contre-productif, comme on le verra plus loin.
