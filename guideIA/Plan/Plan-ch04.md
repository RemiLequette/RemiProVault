# Plan — Chapitre 5

### 5. Le problème du poisson rouge

#### Mots-clés

- stateless
- sans état
- historique
- contexte
- fenêtre de contexte
- token

#### Figures

> 🖼️ **figure** `prompt-etendu-historique`
> Schéma de la structure du message envoyé au modèle :
> [prompt système] + [historique de la conversation] + [dernière question].
> Mettre en évidence que l'historique grossit à chaque échange.

#### Encadrés

> 📦 **encadré** `tokens`
> Les tokens
> Définition des tokens : unité de mesure des LLM, roughly un mot ou un fragment de mot.
> Ordre de grandeur, coût (calcul GPU, donc énergie) — sans jugement de valeur, juste
> les faits qui permettent de comprendre ce que les gens entendent.
> Lien avec la fenêtre de contexte.

#### Contenu

**Les LLM n'ont aucune mémoire**
Aspect fondamental : entre deux générations, le modèle repart de zéro.
Pas de mémoire, pas d'état — stateless. Chaque génération est indépendante.
Le titre du chapitre fait référence à la réputation (humoristique) des poissons rouges
qui oublient tout — plus élégant que d'autres métaphores.

**Le problème pour l'opérateur**
Difficile de soutenir une conversation quand on oublie tout à chaque étape.
Solution : l'opérateur renvoie tout l'historique dans le texte à compléter.
Utiliser la figure prompt-etendu-historique ici.

Ce message complet s'appelle tantôt "prompt", tantôt "contexte" — parce qu'il contient
à la fois la question et les éléments de contexte nécessaires pour y répondre. La note
d'humour sur la double signification de "contexte" est intégrée dans le guide de façon
naturelle — plan aligné sur la structure du guide.

**L'importance de maîtriser la longueur**
Le message envoyé au modèle a une taille limite — la fenêtre de contexte —
qui dépend du LLM. Elle est très grande, mais l'historique croît vite.
Quand on approche de la limite, l'opérateur peut compresser l'historique
(résumer les échanges anciens) pour faire de la place.

Tout ce texte a un coût : c'est ce qu'on appelle les tokens — voir encadré tokens.
(Inclure dans l'encadré : coût énergétique — calcul GPU, donc énergie — sans jugement
de valeur, pour raccrocher ce que les gens entendent aux bons concepts.)

Un contexte très long peut aussi créer de la confusion pour le modèle — on verra
pourquoi dans le chapitre sur l'attention.

**Conclusion pratique**
Des conversations courtes et centrées sur un sujet. Nouvelle session si le sujet change.
(Si toutes les réunions étaient comme ça !)
