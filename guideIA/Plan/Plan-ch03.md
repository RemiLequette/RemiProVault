# Plan — Chapitre 3

## Première Partie

### 3. C'est quoi la génération ?

#### Objectifs

- Expliquer ce qu'est un modèle et comment il génère une réponse — par interpolation, pas par raisonnement
- Faire comprendre d'où viennent les hallucinations : qualité des données et puissance du modèle
- Poser la distinction interpolation / extrapolation comme boussole pratique pour le lecteur
- Introduire l'idée centrale : une IA n'a que des croyances, pas de notion d'avoir tort

#### Mots-clés

- génération
- données_dentraînement
- interpolation
- extrapolation
- hallucination
- underfitting
- overfitting
- raisonnement

#### Figures

> 🖼️ **figure** `immo-regression`
> Graphique axes surface (x) / prix (y). Points représentant les transactions.
> Droite de régression représentant le prix moyen au m². Ligne verticale rouge
> partant de la surface de l'appartement cible, ligne horizontale arrivant à son
> prix estimé. Style pédagogique, épuré.

> 🖼️ **figure** `immo-outliers`
> Mêmes données que immo-regression, avec quelques points aberrants bien exagérés
> (un bien bradé, un vendu très au-dessus du marché). Montrer comment ils dévient
> la droite de régression.

> 🖼️ **figure** `immo-deux-segments`
> Modèle à deux pentes : une pour les petites surfaces (prix/m² élevé), une pour
> les grandes (prix/m² plus faible). Illustre l'enrichissement du modèle par rapport
> à immo-regression.

> 🖼️ **figure** `immo-underfitting`
> Données avec une structure visible (courbe), modèle trop simple (droite) qui ne
> capture pas la tendance. Le modèle "n'est pas assez intelligent".

> 🖼️ **figure** `immo-overfitting`
> Même données, modèle trop complexe dont la courbe passe par tous les points mais
> part dans des directions improbables entre les données. Le modèle "apprend par
> cœur" au lieu de généraliser.

> 🖼️ **figure** `immo-interpolation-extrapolation`
> Zones colorées fond vert/rouge délimitant les données d'entraînement.
> Labels "interpolation" / "extrapolation" de part et d'autre.

#### Encadrés

> 📦 **encadré** `biais-de-positivite`
> Le biais de positivité
> Pourquoi le modèle affirme plutôt qu'il ne doute. La littérature humaine décrit
> rarement ce qu'on ignore — donc le modèle a peu appris à dire "je ne sais pas".
> Effet sur les hallucinations : le modèle comble les blancs avec confiance plutôt
> que d'admettre une limite.

#### Contenu

**Accroche**
Il y a quelque chose de magique dans cette génération : on comprend que la connaissance
vient des données d'entraînement, mais on se doute bien que nulle part dans la littérature
humaine il n'y a un texte aussi tarabiscoté que le verbiage d'un prompt système ! Comment
ça marche alors ?

Ça repose sur le concept d'interpolation. Je vais l'expliquer en partant d'un exemple
volontairement très simple.

**L'exemple immobilier**
Je voudrais estimer la valeur d'un appartement. Je dispose d'une liste de transactions
immobilières avec les surfaces et les prix.

Approche classique : calculer le prix moyen au m², puis multiplier par la surface.

Utiliser la figure immo-regression ici.

Sans le savoir, comme Monsieur Jourdain faisait de la prose, je viens de construire un modèle. Un modèle c'est :

- une procédure pour calculer une sortie à partir d'une entrée : multiplier la surface par le prix moyen au m² (c'est la génération)
- des paramètres qui influencent le calcul : ici un seul, le prix moyen au m²
- des données d'entraînement pour pouvoir calibrer les paramètres : ici les transactions immobilières
- une méthode d'apprentissage pour trouver les paramètres qui reflètent le mieux les données d'entraînement : le calcul du prix moyen au m²

Même si la surface de mon bien ne figure pas dans les transactions historiques, ou qu'il y en a plusieurs avec la même surface et des prix différents, je trouve une valeur qui est une bonne base de réflexion. C'est la magie de l'interpolation.

**Le LLM, même concept, échelle différente**

- Les entrées/sorties : des débuts de texte et leur complétion
- Les données d'entraînement : une immense base de textes, chacun découpé en plusieurs couples (début, complétion)
- La procédure de calcul : c'est la fameuse génération, complexe, mais pas besoin d'entrer dans les détails
- Les paramètres : plusieurs milliards, voire dizaines de milliards
- L'apprentissage : extrêmement complexe, des capacités de calcul gigantesques, ça ne se fait pas tous les jours

**D'où viennent les hallucinations**

On voit tout de suite que notre modèle immobilier peut halluciner et donner des
évaluations plus ou moins irréalistes. Pourquoi ? Deux sources : les données et la puissance du modèle.

*La qualité des données*
Garbage in, garbage out. Valeurs aberrantes — figure immo-outliers ici.

Biais de positivité — encadré `biais-de-positivite` ici.

Biais culturels (anglophonie, Wikipedia vs forums complotistes) et RLHF : un feedback humain qui oriente l'entraînement vers des réponses jugées acceptables, ce qui introduit ses propres biais. Renvoi ch15 pour aller plus loin.

Interpolation vs extrapolation : plus on sort du domaine de compétence, plus les hallucinations sont probables — figure immo-interpolation-extrapolation ici.

*La puissance du modèle*
Les petites surfaces sont souvent plus chères au m² — notre modèle ne capture pas ça. Figure immo-deux-segments ici.

Underfitting : modèle trop simple, ne progresse pas même avec plus d'entraînement — figure immo-underfitting ici.

Overfitting : modèle trop complexe qui apprend par cœur sans généraliser — figure immo-overfitting ici.

**Le raisonnement — ce que l'interpolation ne fait pas**
L'interpolation se fait en une étape. "Justifie ta réponse" ne déclenche pas un raisonnement — ça interpole des schémas de justification. Le modèle reproduit la *forme* d'une démonstration sans en faire le *fond*.

Exemple : la Joconde, les aveugles de Jéricho, les noces de Cana. Le modèle associe des éléments connus (Léonard de Vinci, la Bible, Vinci encore) sans vérifier leur cohérence. Il croit — il n'a pas tort, il n'a pas raison, il interpole.

**Conclusion**
Une IA n'a que des croyances. Elle n'a aucune notion d'avoir tort — ce qui est une notion de raisonnement, pas d'interpolation. C'est le fil directeur de tout ce qui suit.
