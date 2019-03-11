# Prélude: Valeurs et Expressions

*Le matériel suivant est extrêmement basique, mais comme dans la plupart des histoires, la meilleure façon de commencer est de commencer au tout début.*

Imaginez que nous visitons notre café préféré. Ils nous feront à peu près n'importe quelle boisson de notre choix, d'un expresso ristretto intense à un cappuccino sec, en passant par des préparations à base de café au goût de café contenant divers sirops et laits concentrés. (Vous tolérez l’existence de boissons sucrées car elles offrent une marge bénéficiaire suffisante à l’établissement pour que vous puissiez rester toute la journée à procrastiner via la WiFi en commandant une boisson à 1,20 € toutes les quelques heures.)

Vous exprimez votre commande à un bout du comptoir, les gens derrière le comptoir réalisent la magie et vous livrent le café que vous appréciez à l'autre bout. C’est exactement comme cela que fonctionne l’environnement JavaScript aux fins de ce livre. Nous allons nous passer de serveurs Web, navigateurs et autres complexités et traiter ce modèle simple: vous donnez une [expression] à l'ordinateur, elle renvoie une [valeur], exactement comme vous exprimez vos souhaits à un bariste et recevez un café. en retour.

Pous allons utiliser NodeJS, tout simplement en l'installant et en ouvrant une fenêtre de commande, on saisie `node` puis on execute notre code.

[expression]: https://fr.wikipedia.org/wiki/Expression_(informatique)
[valeur]: https://fr.wikipedia.org/wiki/Valeur_(informatique)

## valeurs et expressions

Toutes les valeurs sont des expressions. Dites que vous commandez au bariste un café Cubano. Eh oui, vous remettez une tasse à café. Vous dites : "Je veux une de ces boissons." Le bariste n’est pas idiot, il vous rend la tasse tout de suite et vous obtenez exactement ce que vous voulez. Ainsi, un café cubano est une expression (vous pouvez l'utiliser pour passer une commande) et une valeur (vous la récupérez du bariste).

Essayons ceci avec quelque chose que peut facilement comprendre l'ordinateur :

    42

Est-ce que ceci est une expression ? Une valeur ? Aucune des deux ? Ou les deux ?

La réponse est que c’est à la fois une expression *et* une valeur. [^représentation] Il est très facile de dire que c’est très simple : lorsque vous le saisissez en JavaScript, vous obtenez la même chose, tout comme notre café Cubano :

    42
      //=> 42

[^représentation]: Techniquement, c'est une *représentation* d'une valeur utilisant la notation Base10, mais nous n'avons pas à nous en préoccuper dans ce livre. Vous et moi comprenons tous les deux que cela signifie "42", de même que l'ordinateur.

Toutes les valeurs sont des expressions. C'est facile ! Existe-t-il d'autres types d'expressions ? Biensûre ! Retournons au café. Au lieu de remettre le café fini, nous pouvons remettre les ingrédients. Remettons un peu de café moulu et de l’eau bouillante.

{pagebreak}

A> Les lecteurs astucieux se rendront compte que nous omettons quelque chose. Toutes nos félicitations ! Prenez une gorgée d'expresso. Nous y reviendrons dans un instant.

Maintenant, le bariste nous rend un expresso. Et si nous remettons l'expresso, nous récupérons celui-ci. L'eau bouillante et le café moulu sont donc une expression, mais ce n'est pas une valeur. [^homoiconicity] L'eau bouillante est une valeur. Le café moulu est une valeur. L'expresso est une valeur. L'eau bouillante plus le café moulu est une expression.

[^homoiconicity]: Dans certains langages, les expressions sont une sorte de valeur en elles-mêmes et peuvent être manipulées. Le grand-père de ces langues est Lisp. JavaScript n'est pas un tel langage, les expressions en elles-mêmes ne sont pas des valeurs.

Essayons aussi cela avec quelque chose d'autre que l'ordinateur comprend facilement:

    "JavaScript" + " " + "Allonge"
      //=> "JavaScript Allonge"

Nous voyons maintenant que les "chaînes" sont des valeurs et vous pouvez créer une expression à partir de chaînes et d'un opérateur `+`. Puisque les chaînes sont des valeurs, elles sont aussi des expressions en elles-mêmes. Mais les chaînes avec des opérateurs ne sont pas des valeurs, ce sont des expressions. Maintenant, nous savons ce qui manquait avec notre exemple "café moulu plus eau chaude". Le marc de café était une valeur, l’eau bouillante était une valeur et l’opérateur "plus" entre eux faisait de l’ensemble une expression qui n’était pas une valeur.

## valeurs et identité

En JavaScript, nous testons si deux valeurs sont identiques à l'opérateur `===` et si elles ne sont pas identiques à l'opérateur `!==`:

		2 === 2
			//=> true

		'hello' !== 'goodbye'
			//=> true

Comment fonctionne `===`, exactement? Imaginez que l'on vous montre une tasse de café. Et puis on vous montre une autre tasse de café. Les deux tasses sont-elles "identiques ? " En JavaScript, il y a quatre possibilités :

D'abord, parfois, les tasses sont de différentes sortes. L'un est une demitasse, l'autre une chope. Cela correspond à la comparaison de deux choses en JavaScript ayant différents *types*. Par exemple, la chaîne `"2"` n'est pas la même chose que le nombre `2`. Les chaînes et les nombres étant de types différents, les chaînes et les nombres ne sont jamais identiques:

    2 === '2'
      //=> false

    true !== 'true'
      //=> true

Deuxièmement, parfois, les tasses sont du même type--peut-être deux tasses à expresso--mais leur contenu est différent. L'un tient un simple, l'autre un double. Cela correspond à la comparaison de deux valeurs JavaScript qui ont le même type mais un "contenu" différent. Par exemple, le nombre `5` n'est pas la même chose que le nombre `2`.

    true === false
      //=> false

    2 !== 5
      //=> true

    'two' === 'five'
      //=> false

Que se passe-t-il si les tasses sont du même type *et* que le contenu est le même ? Eh bien, les troisième et quatrième possibilités de JavaScript couvrent cela.

### valeur types

Troisièmement, certains types de tasses ne portent aucune marque distinctive. S'ils sont du même genre de tasse et qu'ils ont le même contenu, nous n'avons aucun moyen de faire la différence entre eux. C'est le cas des chaînes, des nombres et des booléens que nous avons vus jusqu'à présent.

    2 + 2 === 4
      //=> true

    (2 + 2 === 4) === (2 !== 5)
      //=> true

Notez bien ce qui se passe avec ces exemples: même lorsque nous obtenons une chaîne, un nombre ou un booléen à la suite de l'évaluation d'une expression, celle-ci est identique à une autre valeur du même type avec le même "contenu". Les chaînes, les nombres et les booléens sont des exemples de ce que JavaScript appelle les types "valeur" ou "primitif". Nous utiliserons les deux termes de manière interchangeable.

Nous n'avons pas encore rencontré la quatrième possibilité. Pour étirer un peu la métaphore, certains types de tasses ont un numéro de série sur le fond. Ainsi, même si vous avez deux tasses du même type et que leur contenu est identique, vous pouvez toujours les distinguer.

![Cafe Macchiato is also a fine drink, especially when following up on the fortunes of the Azzurri or the standings in the Giro D'Italia](images/macchiato_1200.jpg)

### référence types

Alors, quels types de valeurs pourraient être du même type et avoir le même contenu, mais ne pas être considéré comme identique à JavaScript ? Rencontrons une structure de données très courante dans les langages de programmation contemporains, le *Array* (d'autres langages l'appellent parfois une liste ou un vecteur).

Un tableau ressemble à ceci: `[1, 2, 3]`. Ceci est une expression, et vous pouvez combiner `[]` avec d'autres expressions. Laissez vous aller avec des choses comme :

    [2-1, 2, 2+1]
    [1, 1+1, 1+1+1]

Notez que vous générez toujours des tableaux avec le même contenu. Mais sont-ils identiques de la même manière que chaque valeur de `42` est identique à toute autre valeur de `42` ? Essayez-les par vous-même:

    [2-1, 2, 2+1] === [1,2,3]
    [1,2,3] === [1, 2, 3]
    [1, 2, 3] === [1, 2, 3]

Que diriez-vous de ça! Lorsque vous tapez `[1, 2, 3]` ou l'une de ses variantes, vous saisissez une expression qui génère son propre tableau *unique* qui n'est identique à aucun autre tableau, même si ce dernier ressemble également à `[ 1, 2, 3]`. C'est comme si JavaScript générait de nouvelles tasses de café avec des numéros de série sur le fond.

A> Les tableaux semblent extrêmement simples, mais ce mot "référence" est tellement chargé en possibilités qu'il existe un chapitre entier consacré à la discussion [rebinding and references](#references). Essayez de saisir ce code :
A>
A> <<(code/ouroboros.js)
A>
A> Vous avez juste créé un [ouroborien](https://fr.wikipedia.org/wiki/Ouroboros) array, un array qui se contient lui même.

Ils se ressemblent, mais si vous les examinez avec `===`, vous voyez qu'ils sont différents. Chaque fois que vous évaluez une expression (y compris en tapant quelque chose) pour créer un tableau, vous créez une nouvelle valeur distincte même si elle *semble* être identique à une autre valeur de tableau. Comme nous le verrons, cela est vrai de nombreux autres types de valeurs, notamment les *fonctions*, le sujet principal de ce livre.
