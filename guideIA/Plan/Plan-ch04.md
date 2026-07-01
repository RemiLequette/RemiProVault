# Plan — Chapitre 4

### 4. Le problème du poisson rouge

#### Mots-clés

- stateless
- sans état
- historique
- session
- chat
- fenêtre de contexte
- token

#### Figures

> 🖼️ **figure** `prompt-etendu-historique`
> Structure du prompt étendu : prompt système, historique et dernière question
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

**Le poisson rouge**
Aspect fondamental : entre deux générations, le modèle repart de zéro.
Pas de mémoire, pas d'état — stateless. Chaque génération est indépendante.
Le titre fait référence à la réputation (humoristique) des poissons rouges —
plus élégant que d'autres métaphores, et plus honnête : le poisson rouge
n'oublie pas vraiment tout, mais le LLM, si.

**La solution de l'opérateur**
Difficile de soutenir un travail quand on oublie tout à chaque étape.
Solution : l'opérateur reconstitue à chaque échange un message complet incluant
tout l'historique, et l'envoie au modèle. Le modèle ne se souvient de rien —
mais il reçoit tout.
Utiliser la figure prompt-etendu-historique ici.

**La session**
Ce flux d'échanges reconstitués s'appelle une session — ou un chat dans l'interface.
Assumer le choix terminologique : on dira session plutôt que chat, parce qu'on travaille.
Cohérence avec ch01 "Travailler avec une IA" — la connotation de travail est délibérée.
(chat = bavarder, session = travailler)

**La fenêtre de contexte et les tokens**
Le message envoyé au modèle a une taille limite — la fenêtre de contexte.
Elle est très grande, mais l'historique croît à chaque échange.
Quand on approche de la limite, l'opérateur peut compresser les échanges anciens
pour faire de la place.
Tout ce texte a un coût : c'est ce qu'on appelle les tokens — voir encadré tokens.
Un contexte très long peut aussi créer de la confusion — on verra pourquoi au ch06.

**Conclusion pratique**
Des sessions courtes, centrées sur un seul sujet. Nouvelle session si le sujet change.
La conclusion découle naturellement de la mécanique : plus la session est longue,
plus le contexte grossit, plus le risque de confusion augmente.
(Si toutes les réunions étaient organisées comme ça...)
