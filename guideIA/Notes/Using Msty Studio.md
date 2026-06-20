
# Tools (MCP)

https://www.youtube.com/watch?v=vpIdDka9hLI
https://www.youtube.com/watch?v=5M3zNodkueo

Il faut declarer dans toolbix puis activer en bas de la conversation

npm.gs.com  catalogue de tools, search mcp (exemple n8n)

# Personna

Dynamic variables
Can attach model, tools ...


# Utiliser pour Guide IA

dans un autre domaine, j'ecrit un livre. Je pensais combiner msty et obsidian

C'est une excellente idée d'organisation. Associer Msty Studio et Obsidian est un choix particulièrement intelligent pour un projet d'écriture au long cours. Ces deux outils partagent la même philosophie fondamentale : **le contrôle total des données locales et le format Markdown.**

Puisque Obsidian gère vos fichiers sous forme de texte brut (`.md`) organisés dans un simple dossier (un "Coffre" ou _Vault_), Msty peut interagir avec votre livre exactement de la même manière qu'avec un projet de code informatique.

![Le système Obsidian : des fichiers Markdown locaux interconnectés, généré par IA](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQzH7wke4mfEfdlHpkDAxeUyOu_4ErstZg4BA5q5wuioR6KyVHWY3NABv0c&s=10)

Le système Obsidian : des fichiers Markdown locaux interconnectés. Source : Obsidian Forum

Voici comment structurer cette synergie pour enrichir votre processus d'écriture.

### 1. La synchronisation automatique (Knowledge Stacks)

L'indexation de votre livre dans Msty se fait sans aucun effort de conversion :

- Créez une **Knowledge Stack** dans Msty pointant directement vers le dossier de votre coffre Obsidian (ou un sous-dossier dédié à votre livre).
    
- En activant le **Watch Mode** de Msty, chaque fois que vous ajoutez un chapitre, modifiez une fiche de personnage, ou rédigez une note de recherche dans Obsidian, Msty met à jour sa base de données vectorielle locale en tâche de fond.
    
- Lorsque vous interrogez l'IA dans Msty, celle-ci a instantanément accès à la totalité de votre manuscrit et de vos notes de préparation.
    

### 2. L'exploitation du contexte pour l'écriture

Cette combinaison vous permet de confier des tâches d'analyse complexes à l'IA, basées sur votre propre style et vos propres données :

- **Vérification de la cohérence (Continuity Check) :** Vous pouvez demander à Msty : _"En me basant sur mes notes de recherche et les chapitres 1 à 3, est-ce que la chronologie des événements tient la route ?"_ ou _"Vérifie si le concept technique expliqué au chapitre 5 n'est pas en contradiction avec l'introduction."_
    
- **Gestion des encadrés et notes annexes :** Si votre ouvrage comporte des encadrés méthodologiques, des définitions inline ou des focus spécifiques, vous pouvez demander à Msty de structurer un premier jet d'encadré en lui fournissant vos notes brutes stockées dans Obsidian.
    
- **Analyse stylistique et structurelle :** Vous pouvez lui soumettre un nouveau chapitre et lui demander : _"Est-ce que le ton de cette section reste fidèle au style des chapitres précédents stockés dans ma Knowledge Stack ?"_
    

### 3. Le flux de travail pour la rédaction (Le Forge Mode)

Pour éviter les allers-retours fastidieux de copier-coller :

1. Utilisez le chat de Msty pour brainstormer la structure d'une section ou développer une idée.
    
2. Basculez la réponse générée dans le **Forge Mode** de Msty pour nettoyer le texte, réorganiser les paragraphes et affiner les formulations.
    
3. Enregistrez directement le fichier depuis le Forge Mode dans votre répertoire Obsidian. Obsidian détectera instantanément le nouveau fichier ou la mise à jour, prêt pour votre relecture.
    

### L'architecture idéale de votre coffre Obsidian pour l'IA

Pour que l'indexation de Msty soit la plus fine possible, il est recommandé de structurer votre coffre Obsidian de manière modulaire :

```
Mon_Livre/
├── 01_Manuscrit/       # Un fichier par chapitre (ex: Chapitre_01.md)
├── 02_Preparation/     # Vos fiches de concepts, de personnages ou de décors
└── 03_Documentation/   # Vos sources brutes, recherches et articles copiés
```

Cette granularité permet à Msty d'extraire des _chunks_ (morceaux de texte) très ciblés lors des recherches documentaires, maximisant la pertinence des réponses du modèle (comme Claude ou un modèle local) sans saturer sa fenêtre de contexte. Vous conservez un environnement d'écriture ultra-fluide, totalement privé, et paré pour le long terme.

je n'ai pas besoin d'un MCP filesystem ou obsidian

C'est une excellente simplification de votre environnement de travail. Le formatage natif de ces deux outils rend effectivement l'utilisation d'un serveur MCP complexe totalement superflue.

La force de cette combinaison réside précisément dans le fait qu'Obsidian n'est pas une base de données opaque : ce n'est qu'une interface visuelle posée sur un dossier standard contenant de simples fichiers texte `.md`.

### Pourquoi s'en passer est un excellent choix

- **La puissance native des Knowledge Stacks :** Le moteur de RAG (indexation) intégré par défaut dans Msty Studio est largement suffisant. En lui indiquant simplement le chemin de votre dossier Obsidian, Msty lit, découpe et indexe vos fichiers Markdown de manière native. Il n'a besoin d'aucun intermédiaire technique pour effectuer des recherches croisées dans vos chapitres ou vos notes.
    
- **Zéro configuration, zéro maintenance :** Les serveurs MCP _filesystem_ ou spécifiques à Obsidian demandent souvent de configurer des droits d'accès système, de gérer des jetons d'API locaux ou de lancer des processus en tâche de fond. S'en passer élimine tout risque de bug ou de rupture de compatibilité lors des prochaines mises à jour de Msty.
    
- **Un flux de travail épuré :** Le glisser-déposer de fichiers dans le chat ou l'utilisation du bouton de sauvegarde du _Forge Mode_ suffisent amplement pour transférer les textes générés vers vos dossiers.
    

En restant sur les fonctionnalités de base de Msty pour interroger votre dossier Obsidian, vous conservez une configuration d'écriture ultra-légère, rapide et totalement à l'abri des pannes techniques.
# Live contents
payant