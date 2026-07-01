# Plan — Chapitre 2

## Première Partie

### 2. Le modèle et l'opérateur

#### Objectifs

- Expliquer l'architecture de base d'un système IA : deux composants, deux rôles
- Permettre au lecteur de se repérer dans le paysage des assistants IA qu'il croise au quotidien
- Montrer que la diversité des opérateurs repose sur un petit nombre de LLM
- Poser les enjeux économiques : facturation, open source, commoditisation des modèles

#### Mots-clés

- modèle_de_langage
- LLM
- opérateur
- prompt
- complétion
- prompt_système
- open_source
- agent

#### Figures

> 🖼️ **figure** `flux-opérateur`
> Flux entre l’humain, l’opérateur et le modèle
> Diagramme du flux entre les deux composants :
> humain → prompt → opérateur → prompt étendu → modèle → complétion → opérateur → humain.
> Flux aller et retour distincts entre chaque composant.
> Arrangement vertical, humain en haut, modèle en bas.
> Trois nœuds : humain (gris), opérateur (teal), modèle/LLM (violet).
> Style épuré, flèches directionnelles, labels courts.
> Pas de titre. 

#### Tables

> 📋 **table** `operateurs`
> Les grandes familles d'opérateurs
> Colonnes : Catégorie — Exemples — Usage principal
> Lignes : Chatbots généralistes / Assistants intégrés / Assistants de codage /
> Moteurs de recherche augmentés / Agents (mention courte)
> Objectif : montrer la diversité des opérateurs, pas l'exhaustivité.
> Insister sur le fait que ce sont tous des opérateurs — la diversité est en surface.

> 📋 **table** `LLM`
> Les principaux modèles de langage
> Colonnes : Nom / famille — Éditeur — Propriétaire ou open source — Utilisé par
> Lignes : GPT-4o, Claude, Gemini, Llama, Mistral — exemples illustratifs, pas exhaustifs
> Objectif : montrer qu'il y a peu de LLM sous la diversité des opérateurs.

#### Encadrés

> 📦 **encadré** `prompt-systeme`
> Le prompt système
> Ce que l'opérateur glisse devant votre question sans vous le dire.
> Exemple concret court. Rôle : ton, format, comportement face aux ambiguïtés.
> Secret de fabrication — complexe, propre à chaque opérateur, non documenté.
> Pont vers la bonne formulation des prompts utilisateur : même logique, même bon sens.

> 📦 **encadré** `GPU`
> Serveurs et GPU — pourquoi ça coûte cher
> Le modèle tourne sur une infrastructure lourde : serveurs spécialisés, GPU massivement
> parallèles. Ce n'est pas un logiciel qu'on installe — c'est une usine qu'on loue à
> la requête. Relie le technique à l'économique : coût par appel API, modèle freemium,
> course aux investissements des grands acteurs.

> 📦 **encadré** `open-source`
> Open source et commoditisation des modèles
> Un LLM open source peut être téléchargé, modifié, hébergé par n'importe qui.
> Thèse centrale : les modèles tendent à se banaliser — la valeur se déplace vers
> les opérateurs et les usages. Meta publie Llama : tout le monde peut s'en emparer,
> ce qui casse la rente des grands propriétaires. Implications pour le marché :
> prolifération d'opérateurs, pression sur les prix, découplage modèle / expérience.
> Message au lecteur : le modèle sous le capot compte moins que ce qu'on en fait.

#### Contenu

**Vue d'ensemble — la figure d'abord**
Partir de la figure `flux-opérateur`. Un système IA repose sur deux composants :
un modèle et un opérateur. L'utilisateur ne parle jamais directement au modèle.
Poser les deux noms, les deux rôles en une phrase chacun. Le reste du chapitre
descend dans le détail.

**Le modèle de langage (LLM)**
Rôle : compléter un texte inachevé. Mention courte de l'entraînement — le modèle
a ingéré des milliards de textes pour acquérir cette capacité, on verra comment
au chapitre suivant. Exemples de complétion (capitale de France, équipe de foot).

**L'opérateur**
Rôle : faire l'interface. Reçoit le prompt, l'enrichit, envoie au modèle, affiche
la réponse. Trois noms pour la même chose selon les contextes : opérateur (usage
général), orchestrateur (contexte agents, gestion de plusieurs modèles),
harnais / harness (rôle minimal de simple relais). Note : orchestrateur tend à
désigner des systèmes plus complexes — on y reviendra.
Point clé : c'est l'opérateur qui façonne l'expérience utilisateur. Même LLM,
opérateurs différents — expériences radicalement différentes.
Encadré prompt système ici.

**Infrastructure et économie**
Le modèle tourne sur des serveurs spécialisés (GPU) — encadré dédié.
Double modèle économique : l'opérateur paie le modèle à l'usage (API),
l'utilisateur paie l'opérateur en SaaS (freemium, abonnement).
Les fournisseurs verticalement intégrés (modèle + opérateur) sont moins transparents
sur leur refacturation interne — c'est leur droit, et c'est une source de rumeurs.

**Les familles d'opérateurs — table `operateurs`**
Taxonomie en 5 catégories. Ce sont tous des opérateurs — la diversité est en surface.
Les agents : mention courte, renvoi aux chapitres suivants.

**Les LLM sous le capot — table `LLM`**
Peu de LLM, beaucoup d'opérateurs. Certains LLM sont partagés par des concurrents.
Open source vs propriétaire — encadré dédié. Thèse de la commoditisation.

**Ce que ça change pour l'utilisateur**
Connaître cette architecture aide à comprendre pourquoi deux assistants répondent
différemment à la même question — et pourquoi changer de modèle dans un chatbot
n'est pas anodin. Le modèle sous le capot compte, mais l'opérateur compte autant.
