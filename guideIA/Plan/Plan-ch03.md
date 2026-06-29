# Plan — Chapitre 3

### 3. C'est quoi la génération ?

#### Mots-clés

- génération
- données d'entraînement
- interpolation
- extrapolation
- hallucination
- underfitting
- overfitting

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
- des données d'entrainement pour pouvoir calibrer les paramètres: ici les transactions immobilières
- une méthode d'apprentissage pour trouver les paramètres qui reflètent le mieux les données d'entraînement: le calcul du prix moyen au m2

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
Garbage in, garbage out. Un exemple typique est la présence de valeurs aberrantes — un bien bradé, un vendu très au-dessus du marché — qui vont dérailler le calcul des paramètres.

Utiliser la figure immo-outliers ici.

Biais de positivité. Biais culturels (anglophonie). Wikipedia c'est bien, les forums conspirationnistes c'est moins bien.

RLHF : améliore l'acceptabilité humaine des réponses mais introduit aussi des biais.

Interpolation vs extrapolation : plus on sort de son domaine de compétence, plus il y a des chances de dire n'importe quoi.

Utiliser la figure immo-interpolation-extrapolation ici.

*La puissance du modèle*
Les petites surfaces sont souvent plus chères au m² — notre modèle ne capture pas ça.

Utiliser la figure immo-deux-segments ici.

Underfitting : quand un modèle n'est pas assez puissant, il ne peut pas progresser même avec plus d'entraînement.

Utiliser la figure immo-underfitting ici.

Overfitting : modèle très complexe qui "apprend par cœur" sans généraliser.

Utiliser la figure immo-overfitting ici.

La question du raisonnement : l'interpolation qui se passe en une seule étape n'est pas très compatible avec l'idée de poser des étapes de raisonnement. "Justifie ta réponse" — l'interpolation va s'accrocher aux connaissances où "justifier la réponse" est associé à une structure de phrase. Compliqué, et il ne va pas vraiment raisonner, juste interpoler des schémas.

Exemple Joconde / aveugles de Jéricho / noces de Cana — illustration du mécanisme de croyance sans raisonnement.

Une IA n'a que des croyances et n'a aucune notion "d'avoir tort".
