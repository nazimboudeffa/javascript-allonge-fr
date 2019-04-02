## Ah. J'Aimerais Avoir un Argument, S'Il Vous Plait.[^mp] {#fargs}

[^mp]: [The Argument Sketch](http://www.mindspring.com/~mfpatton/sketch.htm) de "Monty Python's Previous Record" et "Monty Python's Instant Record Collection"

Jusqu'à présent, nous avons examiné des fonctions sans arguments. Nous n'avons même pas dit quel argument *est*, seulement que nos fonctions n'en ont aucun.

A> La plupart des programmeurs sont parfaitement familiers avec les arguments (souvent appelés "paramètres"). Les mathématiques du secondaire parlent de ça. Donc, vous savez ce qu’ils représentent, et je sais que vous savez ce qu’ils sont, mais s'il vous plaît soyez patient avec l’explication !

Faisons une fonction avec un argument :

    function (room) {}

Cette fonction a un argument, `room`, et pas de corps. Voici une fonction avec deux arguments et sans corps :

    function (room, board) {}

Je suis sûr que vous êtes parfaitement à l'aise avec l'idée que cette fonction a deux arguments, `room`, et `board`. Que fait-on avec les arguments ? Les utiliser dans le corps, bien sûr. Qu'est-ce que vous pensez de ceci ?

    function (diameter) { return diameter * 3.14159265 }

C'est une fonction permettant de calculer la circonférence d'un cercle en fonction du diamètre. J'ai lu cela à haute voix comme "Appliquée à une valeur représentant le diamètre, cette fonction *renvoie* le diamètre multiplié par 3.14159265."

Rappelez-vous que pour appliquer une fonction sans arguments, nous avons écrit `(function () {})()`. Pour appliquer une fonction avec un argument (ou des arguments), on place l’argument (ou les arguments) entre parenthèses, comme ceci :

    (function (diameter) { return diameter * 3.14159265 })(2)
      //=> 6.2831853

Vous ne serez pas surpris de voir comment écrire et appliquer une fonction à deux arguments :

    (function (room, board) { return room + board })(800, 150)
      //=> 950

T> ### Un résumé rapide sur les fontions et les corps
T>
T> La façon dont les arguments sont utilisés dans l'expression d'un corps est probablement parfaitement évident pour vous d'après les exemples, surtout si vous avez utilisé un langage de programmation (à l'exception du dialecte BASIC - que je me souviens de mon école secondaire - qui n'autorisait pas les paramètres lorsque vous avez appelé une procédure).
T>
T> Les expressions soit des représentations de valeurs (comme `3.14159265`, `true`, et `undefined`), des opérateurs qui combinent les expressions (comme `3 + 2`), quelques formes spéciales comme `[1, 2, 3]` pour créer des tableaux à partir d'expressions, ou `function (`*arguments*`) {`*body-statements*`}` pour la création de fonctions.
T>
T> Une des déclarations importantes possibles est une déclaration de return. Une déclaration de return accepte toute expression JavaScript valide.
T>
T> Cette définition pauvre est récursive, nous pouvons donc intuitivement (ou utiliser notre expérience avec d'autres langages) que, puisqu'une fonction peut contenir une instruction return avec une expression, nous pouvons écrire une fonction qui renvoie une fonction ou un tableau contenant une autre expression de tableau. . Ou une fonction qui retourne un tableau, un tableau de fonctions, une fonction qui retourne un tableau de fonctions, ainsi de suite :
T>
T> <<(code/f1.js)

### appel par valeur {#call-by-value}

Comme la plupart des langages de programmation contemporains, JavaScript utilise "l'appel par valeur" [evaluation strategy]. Cela signifie que lorsque vous écrivez du code qui semble appliquer une fonction à une expression ou à des expressions, JavaScript évalue toutes ces expressions et applique les fonctions à la ou aux valeurs résultantes.

[evaluation strategy]: http://en.wikipedia.org/wiki/Evaluation_strategy

Donc quand vous écrivez :

    (function (diameter) { return diameter * 3.14159265 })(1 + 1)
      //=> 6.2831853

Ce qui se passe en interne est que l'expression `1 + 1` a été évaluée en premier, résultant en `2`. Puis notre fonction circonférence est appliquée à `2`.[^f2f]

[^f2f]: Nous avons dit que vous ne pouvez pas appliquer une fonction à une expression. Vous *pouvez* appliquer une fonction à une ou plusieurs fonctions. Les fonctions sont des valeurs! Cela a des applications intéressantes, et elles seront explorées beaucoup plus à fond dans la suite [Functions That Are Applied to Functions](#consumers).

### variables et liaisons

À l'heure actuelle, tout semble simple et direct, et nous pouvons passer aux arguments plus en détail. Et nous allons nous frayer un chemin à partir de `function (diameter) { return diameter * 3.14159265 }` jusqu'aux finctions comme :

    function (x) { return (function (y) { return x }) }

A> `function (x) { return (function (y) { return x }) }` semble complètement déjanté, comme si nous apprenions l'anglais langue secondaire et que l'enseignant nous promet que nous utiliserons bientôt des mots comme *anticonsitutianaliarisme*. Outre le désir d'utiliser des mots longs pour paraître impressionnant, cela ne semblera pas attrayant tant que nous ne serons pas disposés à discuter du rôle de l'Église anglicane dans la politique britannique du XIXe siècle.
A>
A> Mais il y a une autre raison d'apprendre le mot *anticonsitutianaliarisme*: Nous pourrions apprendre comment fonctionnent les préfixes et les postfixes dans la grammaire anglaise. C'est la même chose avec `function (x) { return (function (y) { return x }) }`. Il a en lui-même une signification importante et constitue également une excellente excuse pour en savoir plus sur les fonctions qui créent des fonctions, des environnements, des variables, etc.

Afin de parler de la façon dont cela fonctionne, nous devrions nous entendre sur quelques termes (vous les connaissez peut-être déjà, mais vérifions ensemble et synchronisons nos dictionnaires). Le premier `x`, celui dans `function (x) ...`, est un *argument*. Le `y` dans `function (y) ...` est un autre argument. Le second `x`, celui dans `{ return x }`, n'est pas un argument, *c'est une expression référençant une variable*. Les arguments et les variables fonctionnent de la même manière, qu'il s'agisse de `function (x) { return (function (y) { return x }) }`  ou tout simplement `function (x) { return x }`.

Chaque fois qu'une fonction est appelée ("invoqué" signifie "appliqué à zéro argument ou plus"), un nouvel *environnement* est créé. Un environnement est un dictionnaire (éventuellement vide) qui mappe des variables à des valeurs par leur nom. Le `x` dans l'expression que nous appelons une "variable" est lui-même une expression qui est évaluée en recherchant la valeur dans l'environnement.

Comment la valeur est-elle mise dans l'environnement ? Hé bien pour les arguments, c'est très simple. Lorsque vous appliquez la fonction aux arguments, une entrée est placée dans le dictionnaire pour chaque argument. Alors quand on écrit:

    (function (x) { return x })(2)
      //=> 2

Ce qui se passe est ceci :

1. JavaScript analyse tout cela comme une expression composée de plusieurs sous-expressions.
1. Il commence ensuite à évaluer l'expression, y compris l'évaluation des sous-expressions.
1. Une sous-expression, `function (x) { return x }` est évalué à une fonction.
1. Un autre, `2`, est évalué au nombre 2.
1. JavaScript évalue maintenant l'application de la fonction à l'argument `2`. Voici où ça devient intéressant ...
1. Un environnement est créé.
1. La veleur '2' est liée au nom 'x' dans l'environnement.
1. L'expression 'x' (le côté droit de la fonction) est évalué dans l'environnement que nous venons de créer.
1. La valeur d'une variable lorsqu'elle est évaluée dans un environnement est la valeur liée au nom de la variable dans cet environnement, qui est '2'
1. Et c'est ça notre résultat.

Lorsque nous parlons d’environnements, nous utilisons un [unsurprising syntax][json] pour montrer leurs liens : `{x: 2, ...}`. ce qui signifie que l'environnement est un dictionnaire et que la valeur `2` est liée au nom `x`, et qu'il pourrait y avoir d'autres choses dans ce dictionnaire dont nous ne discutons pas pour le moment.

[json]: http://json.org/

### appel par partage

Plus tôt, nous avons distingué les *types de valeur* de JavaScript de ses *types de référence*. À ce moment-là, nous avons examiné comment JavaScript distingue les objets identiques des objets qui ne le sont pas. Il est maintenant temps d'examiner à nouveau la distinction entre les types valeur et référence.

Il existe une propriété que JavaScript conserve de manière stricte: lorsqu'une valeur - n'importe quelle valeur - est transmise en tant qu'argument à une fonction, la valeur liée à l'environnement de la fonction doit être identique à l'original.

Nous avons dit que JavaScript lie les noms aux valeurs, mais nous n'avons pas précisé ce que signifie associer un nom à une valeur. Maintenant, nous pouvons élaborer: Lorsque JavaScript lie un type de valeur à un nom, il en fait une copie et place la copie dans l'environnement. Comme vous vous en souvenez, les types de valeur tels que les chaînes et les nombres sont identiques s'ils ont le même contenu. Donc, JavaScript peut faire autant de copies de chaînes, de nombres ou de booléens qu'il le souhaite.

Qu'en est-il des types de référence? JavaScript ne place des copies de valeurs de référence dans aucun environnement. JavaScript place *références* pour les types de référence dans les environnements, et lorsque la valeur doit être utilisée, JavaScript utilise la référence pour obtenir l'original.

Étant donné que de nombreuses références peuvent partager la même valeur et que JavaScript les transmet sous forme d'arguments, on peut dire que JavaScript implémente la sémantique "appel par partage". L'appel par partage est généralement compris comme une spécialisation d'appel par valeur. Il explique pourquoi certaines valeurs sont appelées types de valeur et d'autres valeurs, appelées types de référence.

Et avec cela, nous sommes prêts à examiner les *fermetures*. Lorsque nous combinons notre connaissance des types de valeur, des types de référence, des arguments et des fermetures, nous comprenons pourquoi cette fonction est toujours évaluée comme suit : `true` peu importe l'argument [^NaNPedantry] vous l'appliquez à :

    function (value) {
      return (function (copy) {
        return copy === value
      })(value)
    }

[^NaNPedantry]: Sauf si l'argument est NaN, qui n'est égal à rien, y compris lui-même
