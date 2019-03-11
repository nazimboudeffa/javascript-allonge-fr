## Aussi court que possible sur les fonctions, mais rien de moins

En JavaScript, les fonctions sont des valeurs, mais elles sont aussi beaucoup plus que de simples nombres, des chaînes ou même des structures de données complexes telles que des arbres ou des maps. Les fonctions représentent les calculs à effectuer. Comme les nombres, les chaînes et les tableaux, elles ont une représentation. Commençons par la fonction la plus simple possible. En JavaScript, cela ressemble à ceci :

    function () {}

C'est une fonction qui n'est appliquée à aucune valeur et ne produit aucune valeur. Comment représentons-nous "aucune valeur" en JavaScript ? Nous le découvrirons dans une minute. Tout d'abord, vérifions que notre fonction est une valeur :

    (function () {})
      //=> [Function]

Quoi !? Pourquoi n'a-t-il pas tapé en retour `function(){}` pour nous ? Cela *semble* enfreindre notre règle voulant que si une expression est aussi une valeur, JavaScript nous restituera la même valeur. Que se passe-t-il? La réponse la plus simple est que, bien que l'interpréteur JavaScript renvoie effectivement cette valeur, son affichage à l'écran est une question légèrement différente. `[Function]` est un choix fait par ceux qui ont écrit Node.js, l'environnement JavaScript hébergeant JavaScript REPL. Si vous essayez la même chose dans un navigateur, vous verrez le code que vous avez tapé.

{pagebreak}

A> Je préférerais autre chose, mais je dois accepter le fait que ce qui nous est dactylographié à l'écran est arbitraire, et tout ce qui compte, c'est qu'il soit quelque peu utile à la lecture par un humain. Mais nous devons comprendre que même si nous voyions `[Fonction]` ou `function(){}`, JavaScript a en interne une fonction complète et appropriée.

### fonctions et identités

Vous vous rappelez que nous avons deux types de valeurs en ce qui concerne l'identité : les types de valeur et les types de référence. Les types de valeur partagent la même identité s'ils ont le même contenu. Les types de référence ne le font pas.

Quel genre sont des fonctions? Essayons. Pour des raisons d'apaisement de l'analyseur JavaScript, nous allons placer nos fonctions entre parenthèses:

    (function () {}) === (function () {})
      //=> false

Comme les tableaux, chaque fois que vous évaluez une expression pour générer une fonction, vous obtenez une nouvelle fonction qui n'est pas identique à une autre fonction, même si vous utilisez la même expression pour la générer. "Fonction" est un type référence.

### appliquer les fonctions

Mettons les fonctions au travail. La façon dont nous utilisons les fonctions consiste à *les appliquer* à zéro ou plusieurs valeurs appelées *arguments*. Tout comme `2 + 2` produit une valeur (dans ce cas,` 4`), appliquer une fonction à zéro ou plusieurs arguments produit également une valeur.

Voici comment nous appliquons une fonction à certaines valeurs en JavaScript: Disons que *fn_expr* est une expression qui, une fois évaluée, produit une fonction. Appelons les arguments *args*. Voici comment appliquer une fonction à certains arguments :

  *fn_expr*`(`*args*`)`

Pour le moment, nous ne connaissons qu'une expression de ce type: `function(){}`, alors utilisons-la. Nous le mettrons entre parenthèses [^ambigu] pour que l’analyseur reste heureux, comme nous l’avons fait précédemment: `(function(){})`. Puisque nous ne lui donnons aucun argument, nous écrirons simplement `()` après l'expression. Alors nous écrivons :

    (function () {})()
      //=> undefined

Qu'est-ce que `undefined` ?

[^ambigus]: Si vous êtes habitué à d'autres langages de programmation, vous avez probablement intériorisé l'idée que parfois les parenthèses sont utilisées pour grouper des opérations dans une expression comme math, et parfois pour appliquer une fonction à des arguments. Sinon ... Bienvenue dans la famille de langages de programmation [ALGOL] !

[ALGOL]: https://fr.wikipedia.org/wiki/Algol_(langage)

### `undefined`

En JavaScript, l’absence de valeur s’écrit `undefined`, ce qui signifie qu’il n’ya pas de valeur. Il reviendra. `undefined` est son propre type de valeur et agit comme un type de valeur :

    undefined
      //=> undefined

Comme les nombres, les booléens et les chaînes, JavaScript peut afficher la valeur `undefined`.

    undefined === undefined
      //=> true
    (function () {})() === (function () {})()
      //=> true
    (function () {})() === undefined
      //=> true

Peu importe comment vous évaluez `undefined`, vous obtenez une valeur identique. `undefined` est une valeur qui signifie "je n'ai pas de valeur". Mais ça reste une valeur :-)

A> Vous pourriez penser que `undefined` en JavaScript est équivalent à `NULL` en SQL. Non. En SQL, deux éléments `NULL` ne sont pas égaux ni ne partagent la même identité, car deux inconnus ne peuvent pas être égaux. En JavaScript, chaque `undefined` est identique à tous les autres `undefined`.

### void

Nous avons vu que JavaScript représente une valeur non définie en tapant `undefined`, et nous avons généré des valeurs non définies de deux manières :

1. En évaluant une fonction qui ne renvoie pas de valeur `(function(){})()`, et;
2. En écrivant `undefined` nous-mêmes.

Il y a une troisième façon, avec l'opérateur `void` de JavaScript. Voir :

    void 0
      //=> undefined
    void 1
      //=> undefined
    void (2 + 2)
      //=> undefined

`void` est un opérateur qui prend n'importe quelle valeur et l'évalue toujours à `undefined`. Donc, lorsque nous voulons délibérément une valeur indéfinie, devrions-nous utiliser les première, deuxième ou troisième formes ? [^Quatrième] La réponse est, utilisez `void`. Par convention, utilisez `void 0`.

La première forme fonctionne mais elle est encombrante. La seconde forme fonctionne la plupart du temps, mais il est possible de la décomposer en réaffectant `undefined` à une valeur différente, ce dont nous discuterons dans [Réaffectation et Mutation](#reassignment). La troisième forme est garantie de toujours fonctionner, c'est ce que nous allons utiliser. [^Void]

[^Quatrième] : Les codeurs expérimentés en JavaScript sont conscients qu'il existe une quatrième façon, en utilisant un argument de fonction. C'était en fait le mécanisme préféré jusqu'à ce que `void` soit devenu banal.

[^void] : Comme un exercise pour le lecteur, nous vous suggérons de demander à votre amical concepteur de langage de programmation de quartier ou à votre expert en matière de facteurs humains d'expliquer pourquoi un mot clé appelé `void` est utilisé pour générer une valeur` undefined` au lieu de les appeler à la fois `void` ou les deux` undefined`. Nous n'en avons aucune idée.

### fonctions sans arguments et leurs corps

Retour à notre fonction. Nous avons évalué ceci :

    (function () {})()
      //=> undefined

Rappelons que nous appliquions la fonction `function(){}` à aucun argument (car il n'y avait rien à l'intérieur de `()`). Alors, comment savons-nous qu'il faut s'attendre à `undefined` ? C'est facile:

Lorsque nous définissons une fonction [^todonamed], nous écrivons le mot `function`. Nous mettons ensuite une liste d'arguments (éventuellement vide), puis nous donnons à la fonction un *corps* entre accolades `{...}`. Les corps de fonction sont des listes (éventuellement vides) *d'instructions* JavaScript séparées par des points-virgules.

Quelque chose comme : { instruction^1^; instruction^2^; instruction^3^; ... ; instruction^n^ }

Nous n'avons pas discuté de ces *instructions*. Qu'est-ce qu'une instruction ?

[^todonamed]: Fonctions appellée TODO: , probablement discuté dans une toute nouvelle section lorsque nous discutons du hissage `var`.

Il existe plusieurs types de déclarations JavaScript, mais le premier type est celui que nous avons déjà rencontré. Une expression est une déclaration JavaScript. Bien qu'elles ne soient pas très pratiques, les fonctions suivantes sont toutes des fonctions JavaScript valides et sont considérées comme indéfinies lorsqu'elles sont appliquées :

    (function () { 2 + 2 })

    (function () { 1 + 1; 2 + 2 })

Vous pouvez également séparer des instructions avec des sauts de ligne. [^asi] La convention est d'utiliser une forme d'indentation cohérente. :

    (function () {
      1 + 1;
      2 + 2
    })

    (function () {
      (function () {
        (function () {
          (function () {
          })
        })
      });
      (function () {
      })
    })

Ce dernier est un doozy, mais puisqu'un corps de fonction peut contenir une instruction, et une instruction peut être une expression, et une fonction est une expression ... Vous voyez l'idée.

[^asi]: Les lecteurs qui suivent les flame-fests sur Internet peuvent être conscients de quelque chose appelé [automatic semi-colon insertion](http://lucumr.pocoo.org/2011/2/6/automatic-semicolon-insertion/). Basiquement, il y a une étape où JavaScript examine votre code et suit certaines règles pour deviner où vous vouliez insérer des points-virgules si vous les omettez. Cette fonctionnalité a été créée à l'origine comme une sorte de correction d'erreur utile. Certains programmeurs font valoir que, puisque cela fait partie de la définition du langage, écrire du code qui l'exploite est un jeu juste. Par conséquent, ils omettent délibérément tout point-virgule que JavaScript leur insère.

Alors, comment pouvons-nous obtenir une fonction pour renvoyer une valeur lorsqu'elle est appliquée? Avec le mot-clé `return` et toute expression :

    (function () { return 0 })()
      //=> 0

    (function () { return 1 })()
      //=> 1

    (function () { return 'Hello ' + 'World' })()
      // 'Hello World'

Le mot-clé `return` crée une instruction return qui termine immédiatement l'application de la fonction et renvoie le résultat de l'évaluation de son expression.

### fonctions qui évaluent aux fonctions

Si une expression qui évalue une fonction est, eh bien, une expression et si une instruction return peut avoir n'importe quelle expression sur son côté droit ... *Peut-on mettre une expression qui évalue une fonction sur le côté droit d'une expression de fonction ?*

Oui :

    function () {
      return (function () {})
    }

C'est une fonction ! C'est une fonction qui, lorsqu'elle est appliquée, est évaluée à une fonction qui, lorsqu'elle est appliquée, est évaluée à `undefined`. [^mouthful] Utilison une terminology plus simple. Au lieu de dire "que quand c'est appliqué, ça évalue à \_\_\_\_\_," nous dirons "donne \_\_\_\_\_." Et au lieu de dire "donne undefined," nous dirons "ne donne rien."

Alors nous avons *une fonction, qui donne une fonction, qui ne donne rien*. également :

    function () {
      return (function () {
        return true
      })
    }

C'est une fonction, qui donne une fonction, qui donne `true`:

    (function () {
      return (function () {
        return true
      })
    })()()
      //=> true

Bien. Nous avons été très intelligents, mais jusqu'à présent, tout cela semble très abstrait. La diffraction d'un cristal est belle et intéressante en soi, mais vous ne pouvez pas nous en vouloir de vouloir lui montrer un usage pratique, comme pouvoir déterminer la composition d'une étoile à des millions d'années-lumière. Alors ... Dans le chapitre suivant, "[J'aimerais Avoir un Argument, S'il Vous Plait](#fargs)," nous allons voir comment faire des fonctions par la pratique.

[^mouthful]: What a mouthful! C’est la raison pour laquelle d’autres langages, particulièrement axées sur les fonctions, proposent des syntaxes telles que `-> -> undefined`
