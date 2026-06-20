## 5. Le problème du poisson rouge

Le poisson rouge a la réputation d'oublier tout toutes les trente secondes. C'est inexact — mais c'est une bonne métaphore pour décrire un aspect fondamental des LLM.

### Pas de mémoire

Entre deux générations, le modèle repart de zéro. Aucune mémoire, aucun état — on dit qu'il est {{def:mk:stateless}} ({{def:mk:sans_état}}). Chaque génération est entièrement indépendante de la précédente.

C'est un choix d'architecture, pas un oubli de conception. Mais ça pose un problème évident : comment tenir une conversation si on oublie tout à chaque réponse ?

### La solution de l'opérateur

L'{{ref:mk:opérateur}} s'en charge. À chaque échange, il reconstruit un message complet incluant tout l'{{def:mk:historique}} de la conversation, et l'envoie au modèle. Le modèle ne se souvient de rien — mais il reçoit tout.

{{def:fig:prompt-etendu-historique}}

Ce message complet s'appelle tantôt "prompt", tantôt "{{def:mk:contexte}}" — parce qu'il contient à la fois votre question et les éléments de contexte nécessaires pour y répondre. C'est la même chose, vue sous deux angles différents. Le contexte, c'est de la mémoire artificielle.

*(Parenthèse involontairement illustrative : ce paragraphe vient d'utiliser le mot "contexte" en lui donnant deux sens légèrement différents — exactement le genre d'ambiguïté que les chapitres suivants vont explorer.)*

### La fenêtre de contexte

Ce message a une taille limite : la {{def:mk:fenêtre_de_contexte}}. Elle dépend du modèle, et elle est très grande — mais l'historique croît à chaque échange. Quand on approche de la limite, l'opérateur peut compresser les échanges anciens pour faire de la place.

Tout ce texte a un coût : c'est ce qu'on appelle les {{def:mk:token}}s — l'unité de mesure des LLM. Roughly un mot ou un fragment de mot.

Un contexte très long peut aussi créer de la confusion pour le modèle. On verra pourquoi dans le chapitre sur l'attention.

### Conclusion pratique

Des conversations courtes, centrées sur un seul sujet. Si le sujet change, commencez une nouvelle session.

*(Si toutes les réunions étaient organisées comme ça...)*
