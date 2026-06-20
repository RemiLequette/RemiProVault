# Guide pratique de l'assistant IA

## Introduction

L'intelligence artificielle n'est pas vraiment nouvelle. C'est une discipline qui remonte aux années soixante, qui a traversé de longs hivers et progressé par sauts — comme beaucoup de sciences. J'y ai été confronté tôt, par mes choix académiques d'abord (l'optimisation et la recherche opérationnelle), puis dans ma carrière, notamment chez ILOG. Jamais expert au sens strict, mais toujours solidement informé. Indépendamment j'ai toujours été très curieux des sciences, et particulièrement la logique, l'informatique et les neurosciences, à l'époque j'avais dévoré Gödel, Escher, Bach de Douglas Hofstadter.

J'ai toujours eu beaucoup de respect pour les travaux menés dans ce domaine. Et un petit doute sur l'ambition. Et une légère suspicion d'imposture sur le nom lui-même.

À chaque avancée, on était impressionné — puis on reprenait ses esprits, et on décidait de mieux préciser ce que « être intelligent » voulait dire, plutôt que de faire une concession à la machine. Ça ressemble au sophisme du vrai Écossais — voir encadré.

{{def:enc:vrai-ecossais}}
> ### Le sophisme du vrai Écossais
> *L'IA vous explique*
>
> Un sophisme est un raisonnement qui *semble* valide mais ne l'est pas — une faille logique habillée en argument. Celui du vrai Écossais a été formalisé par le philosophe britannique Antony Flew en 1975 : « Aucun Écossais ne mettrait de sucre dans son porridge. » On lui cite un Écossais qui en met. Réponse : « Aucun *vrai* Écossais ne ferait ça. » La définition se rétrécit pour exclure le contre-exemple. La thèse reste invincible — parce qu'elle est devenue indéfendable.
>
> L'IA en a été victime de façon presque mécanique.
>
> - **2011.** Watson remporte Jeopardy! face aux meilleurs joueurs humains. Verdict : comprendre des jeux de mots et des allusions culturelles, ce n'est que de la recherche dans une base de données. Pas vraiment intelligent.
> - **2016.** AlphaGo bat le champion du monde de Go — un jeu réputé intuitif, que les ordinateurs n'étaient pas censés maîtriser de sitôt. Verdict : la stratégie de jeu n'est pas de l'intelligence générale.
> - **Aujourd'hui.** Les grands modèles de langage rédigent, raisonnent, traduisent, codent. Le débat continue — et c'est légitime. Mais il vaut mieux en connaître le mécanisme pour y participer lucidement.
>
> Quant au test de Turing, il est peut-être lui aussi un faux Écossais. Si les LLM finissent par le passer, redéfinira-t-on encore une fois ce que signifie « vraiment » comprendre et être intelligent ?

Il y avait pourtant un consensus sur ce qui manquait à ces succès : le langage. De nombreuses recherches sur la traduction automatique, sans progrès réels, et le sentiment qu'il faudrait une innovation plus générale pour y arriver.

Puis il y a eu le « moment ChatGPT », et l'emballement qui a suivi. OpenAI, Anthropic, Gemini, Mistral, DeepSeek... Les chatbots intelligents se sont répandus dans le grand public, les entreprises ont commencé à expérimenter, une immense vague de bruit, de craintes et d'espoirs a déferlé.

On peut vraiment dire — et j'en suis intimement convaincu — qu'ils ont « craqué » quelque chose cette fois. Quoi exactement ? C'est encore difficile à définir, et j'espère que ce guide vous aidera à vous faire une opinion. Ce qui est sûr, c'est que le fameux test de Turing — « vous ne feriez pas la différence au téléphone » — est clairement en train de rompre. Ce débat, aussi passionnant soit-il, ne retire rien au fait que la technologie est là, prodigieusement efficace, et qu'elle ressemble au début d'une révolution du type de la vapeur, de l'électricité, ou de l'Internet.

Donc je suivais l'actualité, je jouais avec la bête avec enthousiasme, je me tenais informé sur la technologie. Je connaissais déjà l'apprentissage profond qui est sous-jacent — et j'y avais été confronté dans mon travail, notamment dans la planification de la chaîne d'approvisionnement, où la prévision de la demande joue un rôle crucial.

Puis j'ai eu l'opportunité de mener un vrai projet professionnel avec ces outils. C'était au moment où on commençait à les appliquer à l'écriture de logiciel — ce qui ne se réduit pas à aligner du code. J'ai toujours été passionné par la programmation et l'ingénierie logicielle : bases théoriques, technologie, architecture, conduite de projet. Passé par la R&D et le conseil, j'étais soit directement impliqué, soit en forte coopération avec les équipes concernées.

Je me lance donc. Et j'essaie aussi de mieux comprendre comment ça fonctionne en pratique. Je fais partie de ceux qui ne veulent pas d'une baguette magique — qui aiment comprendre l'outil pour tenir compte de ses forces et de ses faiblesses. Par « comprendre », j'entends quelque chose de précis : il n'est pas nécessaire de connaître la mécanique au sens de l'École Polytechnique pour comprendre comment fonctionne une automobile. C'est très utile aux ingénieurs qui la conçoivent. Ça n'apporte pas grand-chose pour l'utiliser correctement et ne pas être complètement démuni face à un garagiste. Et, paradoxalement, ça peut être très difficile d'expliquer clairement, sans jargon ni concepts compliqués. Comme l'a dit Blaise Pascal : « j'ai fait long car je n'ai pas eu le temps de faire court. »

Je suis également agacé — peut-être injustement — par ceux que j'appelle les docteurs du prompt, qui vous fournissent de belles formules alambiquées : « Tu es un professeur de claquettes borgne, pratiquant la peinture à l'huile de façon modérée... » Je ne nie pas que certaines formules fonctionnent, parfois. Mais ça ressemble à des incantations. Des recettes de cuisine sans recette. Tous les chercheurs s'accordent sur le fait que l'art du prompt est très surévalué — et on verra bien pourquoi. Tenons-nous en aux vrais experts, comme Boileau dans l'Art poétique : « ce qui se conçoit bien s'énonce clairement, et les mots pour le dire viennent aisément. »

Et là, il y a une très bonne nouvelle pour ceux d'entre vous qui ne sont pas informaticiens : vous avez un avantage. Vous n'avez pas à désapprendre le mode de communication avec les machines. Les techniques classiques pour les humains fonctionnent très bien.

J'ai donc fait cet effort de compréhension, progressivement. Double surprise : ma productivité a augmenté à des niveaux que je n'aurais pas imaginés, et il y a un réel plaisir à travailler de cette façon. C'est ce que je veux partager.

Je confesse un trait de caractère qui m'a beaucoup aidé : je suis paresseux. Pas le genre qui cherche à ne rien faire — le genre qui cherche à minimiser l'effort pour arriver au résultat, tout en ayant l'amour du travail bien fait. Un {{def:mk:fénéant-créatif}}, si vous voulez. J'ai des idées sur les bonnes pratiques, je vois souvent comment les améliorer — mais je n'arrive pas à être rigoureux parce que ça interrompt le *flot*. Vous connaissez sûrement cette zone où le travail est fluide et créatif, où les idées arrivent en parallèle, où on se dit « tiens, ce serait mieux de faire comme ça » — mais où s'arrêter pour documenter casserait tout. Avec une IA, j'ai trouvé comment tout intégrer au flot. C'est ça, le vrai sujet de ce guide.

Alors je suis là, je commence à barboter dans l'eau, et je me tourne vers ceux qui sont encore au bord et je leur dit: Venez! Plongez! Elle est trop bonne!

*Une note de périmètre : ce guide traite du fonctionnement et de l'utilisation des assistants IA. Il ne couvre pas les aspects légaux, contractuels ou de confidentialité des données, ni des impacts économiques, sociétaux et civilisationnels — sujets importants, mais qui méritent leurs propres références.*

Alors que j'écrivais j'ai développé deux intuitions, et je me suis dit que c'était important de les partager dès l'introduction, tant pis si ca casse le suspense, mais c'est deux excellentes nouvelles.

- "Intelligence Artificielle" est bien un oxymoron, nous avons trouvé l'écossais. Nous sommes tous plus intelligent que l'IA parce que nous sommes rééls, c'est essentiellement couvert dans la conclusion.
- Nous savons tous déjà nous en servir, comme Monsieur Jourdain. Pourquoi, parce que c'est un outil copié, avec amplifications et déformations, mais bien copiés sur l'outil que nous utilisons le plus souvent: notre cerveau. Et cà, c'est couvert par tout le guide.

Ce guide est organisé en trois parties. La première se suffit à elle-même : elle pose le cadre, soulève le capot juste ce qu'il faut pour comprendre d'où viennent les forces et les limitations, puis donne les clés pratiques pour en tirer parti. La deuxième est un ensemble de cas concrets tirés de mon expérience. La troisième, pour ceux qui veulent vraiment démonter le moteur, va beaucoup plus loin sous le capot — mais le guide est complet sans elle.

Ah, j'oubliais. Vous avez croisé le premier encadré de ce guide quelques pages plus haut, une IA l'a rédigé. On y reviendra au chapitre 10, mais je peux déjà vous dire que tous les encadrés sont rédigés exclusivement par un assistant IA, avec un prompt bien choisi. Et un autre petit divulgachage (spoiler en Québecois) la troisième partie est entièrement constituée de conversations socratiques avec des IA. La maïeutique de Socrate, qui est en gros l'art de vous tirer les vers du nez sur des choses que vous savez et croyez que vous ne savez-pas semble avoir été concue spécialement pour les grands modèles de langage (LLM - Large Language Models).

{{def:enc:encadres-ia}}
> ### Les encadrés de ce guide ont été rédigés par une IA
> *L'IA vous explique*
>
> Tous les encadrés que vous lirez dans ce guide ont été rédigés exclusivement par un assistant IA, à partir d'un prompt dédié, co-produit lui aussi avec une IA. Celui-ci ne fait pas exception.
>
> Le prompt en question donne, entre autres, à l'assistant un rôle, une règle d'or, et trois consignes :
>
> *Rôle :* « Tu es un vulgarisateur pragmatique pour professionnels, co-auteur d'un ouvrage destiné à aider à mieux travailler avec l'IA. »
>
> *Règle d'or :* « Un bon encadré apporte un complément utile, mais il doit être étanche. Si le lecteur le saute, il doit pouvoir comprendre le reste du chapitre sans aucun problème. »
>
> *Consignes :* direct, accessible, concret — pas de jargon inutile, pas de fioritures. Court et visuel. Et surtout : identifier l'enjeu concret pour le lecteur, pas seulement la théorie.
>
> Ce guide illustre ce qu'il enseigne. Le chapitre 10 revient en détail sur cette collaboration — et sur ce que ça donne, vraiment, en pratique.

Bonne lecture, dans le flot.
