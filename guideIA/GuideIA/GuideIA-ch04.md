## 4. Le problème du poisson rouge

Le poisson rouge a la réputation d'oublier tout toutes les trente secondes. C'est inexact — le poisson rouge a en réalité une mémoire tout à fait correcte. Mais c'est une bonne métaphore pour décrire un aspect fondamental des LLM, qui eux, oublient vraiment tout.

### Pas de mémoire

Entre deux générations, le modèle repart de zéro. Aucune mémoire, aucun état — on dit qu'il est {{def:mk:stateless}} ({{def:mk:sans_état}}). Chaque génération est entièrement indépendante de la précédente.

Imaginez un call center. Peu importe l'agent qui décroche, il peut traiter votre appel — parce que le système ne dépend pas de lui. N'importe quel appel peut être routé vers n'importe quel agent disponible. C'est ce qui permet au call center de fonctionner à grande échelle, sans jamais bloquer sur un seul interlocuteur. Cette architecture n'est pas spécifique à l'IA — c'est une bonne propriété fondamentale des systèmes distribués, appliquée ici aux LLM.

L'agent ne se souvient pas de vous. Mais si on lui fournit votre dossier, il peut reprendre exactement là où le dernier appel s'est arrêté.

### Le problème concret

Sans dossier, sans historique, le modèle ne sait pas de quoi vous parlez. Prenons un exemple simple :

> — Quelle est la capitale de la France ?
> — Paris.
> — Combien a-t-elle d'habitants ?

Pour vous, *elle* renvoie évidemment à Paris. Pour le modèle, cette deuxième question arrive seule, sans contexte — il ne sait pas ce que *elle* désigne. L'{{ref:mk:opérateur}} doit lui fournir l'historique pour que la conversation ait un sens.

### La solution de l'opérateur

L'opérateur s'en charge. À chaque échange, il reconstitue un message complet incluant tout l'{{def:mk:historique}} de la session, et l'envoie au modèle. C'est l'équivalent de récapituler le dossier à l'agent avant chaque appel — sauf que c'est automatique, invisible, et que vous n'avez rien à faire.

Le modèle ne se souvient de rien — mais il reçoit tout.

{{def:fig:prompt-etendu-historique}}

### La session

Ce flux d'échanges reconstitués s'appelle une {{def:mk:session}}, ou un {{def:mk:chat}} dans l'interface. Les deux termes coexistent, et on les emploiera tous les deux. Mais on dira de préférence *session* — parce qu'on travaille. *Chat* connote la discussion légère ; *session* connote le travail. C'est cohérent avec ce qu'on a dit dès le premier chapitre.

### La fenêtre de contexte et les tokens

Ce message reconstitué a une taille limite : la {{def:mk:fenêtre_de_contexte}}. Elle dépend du modèle, et elle est très grande, mais l'historique croît à chaque échange. Quand on approche de la limite, l'opérateur peut compresser les échanges anciens pour faire de la place. Les premières pages du dossier sont remplacées par un résumé.

Tout ce texte a un coût : c'est ce qu'on appelle les {{def:mk:token}}s.

{{def:enc:tokens}}

> **Les tokens**
> *L'IA vous explique*
>
> Un LLM raisonne sur des nombres — pas des mots. Pour passer du texte aux nombres, il faut découper le texte en unités élémentaires et associer un identifiant numérique à chacune. Ces unités, ce sont les tokens.
>
> Un token ne correspond pas exactement à un mot. Prenez le verbe *manger* :
> - "mang-" et "-er" sont deux tokens distincts
> - "mange", "mangeons", "mangeait" partagent la même racine tokenisée
>
> Ce découpage permet au modèle de reconnaître la parenté entre les formes d'un même verbe, plutôt que de les traiter comme des mots entièrement différents. En pratique, comptez en gros un token par mot courant.
>
> Tout ce qui transite par le modèle est compté en tokens : votre question, l'historique de la session, le prompt système, la réponse générée. La fenêtre de contexte est exprimée en tokens. Les modèles actuels en acceptent plusieurs centaines de milliers.
>
> Les tokens ont un coût réel. Chaque token mobilise des ressources de calcul GPU, donc de l'énergie. C'est pour ça que les opérateurs facturent à l'usage et que les sessions longues coûtent plus cher. Quand vous entendez parler du "coût" d'un LLM, c'est de ça qu'il s'agit.

Un contexte très long peut aussi créer de la confusion pour le modèle. On verra pourquoi au chapitre 6.

### Conclusion pratique

Des sessions courtes, centrées sur un seul sujet. Si le sujet change, nouvelle session.

La logique est simple : plus la session s'allonge, plus le coût augmente et le contexte grossit, plus le risque de confusion augmente. Ce n'est pas une règle arbitraire — c'est une conséquence directe de la mécanique.
Des sessions ni trop courtes, ni trop longues, on retrouve Boucles d'Or.

*(Si toutes les réunions étaient organisées comme ça...)*
