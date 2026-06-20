# Plan — Chapitre 7

### 7. Le problème de l'attention

#### Mots-clés

- attention
- self-attention
- fenêtre de contexte
- dilution
- lost in the middle
- polysémie

#### Figures

> 🖼️ **figure** `attention-dilution`
> Illustration du mécanisme de dilution de l'attention : un contexte court où
> l'attention est concentrée sur les éléments pertinents, vs un contexte long
> où elle se disperse sur de nombreux éléments peu pertinents.

#### Encadrés

> 📦 **encadré** `polysemie`
> La polysémie
> Pourquoi le langage est intrinsèquement ambigu.
> Exemples détaillés autour du mot "tour" et d'autres cas parlants.
> Lien avec le mécanisme d'attention qui résout cette ambiguïté.

#### Contenu

*Note : chapitre en cours — contenu validé en session, rédaction à compléter.*

**L'insight fondateur**
Le langage est ambigu par nature — c'est sa caractéristique fondamentale, pas un défaut.
Le sens d'un mot dépend des autres mots autour de lui. *Tour* peut être la Tour Eiffel,
le Tour de France, un tour de potier, une pièce aux échecs, un tour de chant, un tour de vis,
ou l'ordre dans une file — et on n'a même pas mentionné le donjon médiéval. "Il grimpe dans
les tours" : régime moteur ou quelqu'un qui s'énerve ?

Voir encadré polysemie ici.

C'est précisément le mécanisme qui a débloqué la technologie des LLM : l'**attention**.
Chaque mot "regarde" tous les autres et calcule à quel point ils sont pertinents pour lui.
C'est ce qui permet de résoudre l'ambiguïté — pas mot à mot, mais en pondérant l'ensemble
du contexte simultanément.

**Ce que ça coûte**
Ce mécanisme est puissant mais gourmand : son coût de calcul croît très vite avec la longueur
du contexte. Plus le contexte est long, plus le calcul est lourd — et plus la qualité peut
se dégrader.

**Le problème de la dilution**
Un contexte long ne signifie pas une meilleure compréhension. L'attention doit se répartir
sur l'ensemble du texte — et si ce texte contient beaucoup d'éléments peu pertinents,
la concentration se dilue. C'est contre-intuitif : on a l'impression qu'en donnant plus
d'informations, on aide. En réalité, on peut nuire.

Utiliser la figure attention-dilution ici.

Analogie : un jury qui doit trancher un litige. Lui soumettre 10 documents clairs est plus
efficace que 200 pièces dont 190 sont hors sujet. La pertinence prime sur le volume.

**Le "lost in the middle"**
Dans un contexte très long, les informations placées au milieu sont statistiquement moins
bien traitées que celles en début ou en fin. Implication pratique : si vous avez une
information critique, ne la noyez pas au milieu d'un long document.

**Self-attention et polysémie — dans les deux sens**
Le mécanisme d'attention résout la polysémie. Mais il le fait dans les deux sens : un
contexte mal structuré, traitant plusieurs sujets à la fois, peut introduire de nouvelles
ambiguïtés. Le modèle peut mélanger des fils pourtant distincts. Raison supplémentaire
pour la règle d'or : une session, un sujet.

*Note : cette section est présente dans le guide mais absente du plan — ajoutée ici.*

**Conclusion pratique** (fusionnée avec la section dilution dans le guide)

- Des prompts courts et ciblés
- Les informations importantes en tête ou en queue de contexte
- Ne pas injecter de documents entiers si seules quelques sections sont pertinentes
- Reconnaître les signes de dégradation : réponses qui ignorent des instructions,
  incohérences avec des éléments donnés plus tôt
