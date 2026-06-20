# Session 2026-06-02e — Plan chapitre 7 (en cours)

*Snapshot partiel — session arrêtée avant finalisation du chapitre.*

## Contexte

Session dédiée au plan du chapitre 7 : "Le problème de l'attention". Dernier chapitre
encore vide dans `Plan.md`, et sujet charnière du guide : expliquer le mécanisme
qui a rendu les LLM possibles, et pourquoi ce même mécanisme crée des limites pratiques.

## Ce qui a été produit

Structure et contenu du chapitre 7 validés et écrits dans `Plan.md` :
- L'insight fondateur : ambiguïté du langage, mécanisme d'attention
- Le coût du mécanisme
- Le problème de la dilution (analogie du jury)
- Le "lost in the middle"
- Conclusion pratique

Un encadré technique ajouté en Todo : polysémie — pourquoi le langage est intrinsèquement ambigu.

## Échanges notables

**L'exemple "bank of the river" recadré**

La proposition initiale utilisait l'exemple canonique de la désambiguïsation : "la banque
sur laquelle il s'est assis". L'utilisateur l'a immédiatement recadré — exemple omniprésent,
traduction française laborieuse, et surtout : trop visible comme copié-collé de documentation
technique.

> U : *"l'example 'bank of the river' est partout, la traduction francaise assez malheureuse !"*

**"Tour" : un exemple taillé pour le guide**

L'utilisateur propose "tour" comme exemple de polysémie. Richesse inattendue : Tour Eiffel,
Tour de France, tour de potier, tour aux échecs, tour de chant, tour de vis, c'est mon tour...
et le donjon médiéval absent de la liste — ce qui est en soi une illustration du biais
statistique du modèle (il choisit les acceptions les plus fréquentes dans ses données
d'entraînement).

"Il grimpe dans les tours" : régime moteur ou quelqu'un qui s'énerve ? Double ambiguïté
entre deux sens figurés — sans même avoir besoin du sens propre. Et écho amusant avec
"il a pris la mouche", conservé pour un autre passage du chapitre.

> U : *"fait une liste et remarque qu'on inclut meme pas la bonne vielle tour du chateau-fort"*

**Décision : pas de "transformer" dans ce chapitre**

L'utilisateur a explicitement demandé de ne pas mentionner l'architecture transformer.
Le chapitre reste sur l'intuition (pondération mutuelle des mots) sans jamais entrer
dans la mécanique interne. Cohérent avec le ton pédagogique du guide.

## Ce qui reste à faire

- Affiner les articulations avec les chapitres 5 (fenêtre de contexte) et 6 (contexte)
- Développer l'encadré technique polysémie
- Vérifier le lien "lost in the middle" (Liu et al., 2023) : citer comme recherche ou
  présenter comme observation pratique ?
- Travailler le chapitre 8 (Conclusion)
