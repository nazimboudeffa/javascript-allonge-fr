## Closures and Scope {#closures}

Il est temps de voir comment une fonction dans une fonction fonctionne :

    (function (x) {
      return function (y) {
        return x
      }
    })(1)(2)
      //=> 1

Tout d'abord, utilisons ce que nous avons appris ci-dessus. Étant donné `(`*une fonction*`) (`*un argument*`)`, nous savons que nous faisons un bind de la fonction à l'argument, créons un environnement, lions la valeur de l'argument au nom et évaluons l'expression de la fonction . Donc, nous le faisons d'abord avec ce code :

    (function (x) {
      return function (y) {
        return x
      }
    })(1)
      //=> [Function]

L'environnement appartenant à la fonction portant la signature `function (x) ...` devient `{x: 1, ...}` et le résultat de l'application de la fonction est une autre valeur de la fonction. Il est logique que la valeur du résultat soit une fonction, car l'expression du corps de `function (x) ...` est la suivante :

      function (y) {
        return x
      }

Alors maintenant, nous avons une valeur représentant cette fonction. Ensuite, nous allons prendre la valeur de cette fonction et l'appliquer à l'argument `2`, quelque chose comme ceci :

      (function (y) {
        return x
      })(2)

Il semble donc que nous obtenons un nouvel environnement `{y: 2, ...}`. Comment l'expression `x` va-t-elle être évaluée dans l'environnement de cette fonction ? Il n'y a pas de `x` dans son environnement, il doit venir d'ailleurs.

A> C’est d’ailleurs l’une des caractéristiques déterminantes de JavaScript et des langages de la même famille : S’ils permettent à des choses telles que des fonctions de s’emboîter les unes dans les autres et, dans l’affirmative, comment elles gèrent les variables "de fonction qui sont référencés à l'intérieur d'une fonction. Par exemple, voici le code équivalent en Ruby :
A>
A> <<(code/k.rb)
A>
A> Maintenant, profitons d'un Rallongé détendu avant de continuer!

### Si les fonctions sans variables libres sont pures, les fermetures sont-elles impures ?

La fonction `function (y) {return x}` est intéressante. Il contient une *variable libre*, `x`. [^nonlocal] Une variable libre est une variable qui n'est pas liée à la fonction. Jusqu'à présent, nous n'avons vu qu'un moyen de "faire un bind" à une variable, à savoir en passant un argument portant le même nom. Puisque la fonction `function (y) {return x}` n'a pas d'argument nommé `x`, la variable` x` n'est pas liée à cette fonction, ce qui la rend "libre".

[^nonlocal]: Vous avez pu également entendre le terme "variable non locale". [Les deux sont corrects.](https://en.wikipedia.org/wiki/Free_variables_and_bound_variables)

Maintenant que nous savons que les variables utilisées dans une fonction sont liées ou libres, nous pouvons bifurquer les fonctions dans celles avec des variables libres et celles sans :

  * Les fonctions ne contenant aucune variable libre sont appelées *fonctions pures*.
  * Les fonctions contenant une ou plusieurs variables libres sont appelées *fermetures*.

Les fonctions pures sont les plus faciles à comprendre. Ils signifient toujours la même chose, peu importe où vous les utilisez. Voici quelques fonctions pures que nous avons déjà vues :

    function () {}

    function (x) {
      return x
    }

    function (x) {
      return function (y) {
        return x
      }
    }

La première fonction ne contient aucune variable, donc aucune variable libre. La seconde n'a pas de variables libres, car sa seule variable est liée. Le troisième est en fait deux fonctions, l'une dans l'autre. `function (y) ...` a une variable libre, mais l'expression entière se réfère à `function (x) ...`, et elle n'a pas de variable libre: la seule variable dans son corps est `x`, qui est certainement lié à `function (x) ...`.

Nous en apprenons quelque chose: une fonction pure peut contenir une fermeture.

X>Si les fonctions pures peuvent contenir des fermetures, une fermeture peut-elle contenir une fonction pure? En utilisant uniquement ce que nous avons appris jusqu'à présent, essayez de composer une fermeture contenant une fonction pure. Si vous ne pouvez pas, expliquez pourquoi c'est impossible.

Les fonctions pures signifient toujours la même chose parce que toutes leurs "entrées" sont entièrement définies par leurs arguments. Pas si avec une fermeture. Si je vous présente cette fonction pure `function (x, y) {return x + y}`, nous savons exactement ce qu'elle fait avec `(2, 2)`. Mais qu'en est-il de cette fermeture : `function (y) {return x + y}`? Nous ne pouvons pas dire ce qu'il fera avec l'argument `(2)` sans comprendre la magie pour évaluer la variable libre `x`.

### c'est toujours l'environnement

Pour comprendre comment les fermetures sont évaluées, nous devons revisiter les environnements. Comme nous l'avons dit précédemment, toutes les fonctions sont associées à un environnement. Nous avons également agité quelque chose lorsque nous avons décrit notre environnement. Rappelez-vous que nous avons dit que l'environnement pour `(function (x) {return (function (y) {return x})}) (1)` est `{x: 1, ...}` et que l'environnement pour `( function (y) {return x}) (2)` est `{y: 2, ...}` ? Remplissons les blancs!

L’environnement de `(fonction (y) {return x}) (2)` est *actuellement* `{y: 2, '..': {x: 1, ...}}`. `'..'` signifie quelque chose comme "parent" ou "enceinte" ou "super-environnement". C'est l'environnement de `function (x) ...`, car la fonction `function (y) {return x}` est dans le corps de `function (x) ...`. Ainsi, chaque fois qu'une fonction est appliquée à des arguments, son environnement a toujours une référence à son environnement parent.

Et maintenant, vous pouvez deviner comment nous évaluons `(function (y) {return x}) (2)` dans l'environnement `{y: 2, '..': {x: 1, ...}}`. La variable `x` n'est pas dans l'environnement immédiat de `function (y) ...`, mais elle se trouve dans l'environnement de son parent, elle est donc évaluée à `1` et c'est ce que `(function (y) {return x }) (2)` retourne même si elle a fini par ignorer son propre argument.

A> `function (x) { return x }` est appelée Combinator ou Identity Function. `function (x) { return (function (y) { return x }) }` est appelée le K Combinator ou Kestrel. Certaines personnes sont tellement excitées par cela qu'elles écrivent des livres entiers à leur sujet, certains sont géniaux [génial][mock], ce--pendant devrais-je mettre ceux-là--sont [intéressant][intéressantes] si vous utilisez Ruby.

[mock]: http://www.amzn.com/0192801422?tag=raganwald001-20
[intéressant]: https://leanpub.com/combinators "Kestrels, Quirky Birds, and Hopeless Egocentricity"

Functions can have grandparents too:

    function (x) {
      return function (y) {
        return function (z) {
          return x + y + z
        }
      }
    }

Cette fonction fait à peu près la même chose que :

    function (x, y, z) {
      return x + y + z
    }

Seulement vous l'appelez avec `(1)(2)(3)` au lieu de `(1, 2, 3)`. L'autre grande différence est que vous pouvez l'appeler avec `(1)` et obtenir une fonction que vous pourrez appeler plus tard avec `(2)(3)`.

{pagebreak}

A> La première fonction est le résultat de [currying] la deuxième fonction. L'appel d'une curried fonction avec seulement certains de ses arguments est parfois appelé [application partielle]. Certains langages de programmation sont automatiquement curry et évaluent partiellement des fonctions sans qu'il soit nécessaire de les imbriquer manuellement.

[currying]: https://en.wikipedia.org/wiki/Currying
[application partielle]: https://en.wikipedia.org/wiki/Partial_application

### shadowy variables from a shadowy planet

Une chose intéressante se produit lorsqu'une variable porte le même nom que la variable d'un environnement ancêtre. Considérerons :

    function (x) {
      return function (x, y) {
        return x + y
      }
    }

La fonction `fonction (x, y) {return x + y}` est une fonction pure, car son `x` est défini dans son propre environnement. Bien que son parent définisse également un `x`, il est ignoré lors de l'évaluation de `x + y`. JavaScript recherche toujours une liaison en commençant par l'environnement propre des fonctions, puis par chaque parent jusqu'à ce qu'il en trouve un. La même chose est vraie de :

    function (x) {
      return function (x, y) {
        return function (w, z) {
          return function (w) {
            return x + y + z
          }
        }
      }
    }

When evaluating `x + y + z`, JavaScript will find `x` and `y` in the great-grandparent scope and `z` in the parent scope. The `x` in the great-great-grandparent scope is ignored, as are both `w`s. When a variable has the same name as an ancestor environment's binding, it is said to *shadow* the ancestor.

Lors de l'évaluation de `x + y + z`, JavaScript trouvera `x` et `y` dans la portée des arrière-grands-parents et `z` dans la portée du parent. Le `x` dans la portée arrière-arrière-arrière-grand-parent est ignoré, comme le sont les deux `w`. Lorsqu'une variable porte le même nom que la liaison d'un environnement ancêtre, on dit qu'elle *shadow* l'ancêtre.

C'est souvent une bonne chose.

### qui est venu en premier, l'œuf ou la poule ?

Ce comportement de fonctions pures et de fermetures a de très nombreuses conséquences qui peuvent être exploitées pour écrire un logiciel. Nous allons les explorer en détail et examiner certains des autres mécanismes fournis par JavaScript pour travailler avec des variables et des états mutables.

Mais avant cela, il y a une dernière question: où commence l'ascendance? S'il n'y a pas d'autre code dans un fichier, qu'est-ce que l'environnement parent de `function (x) {return x}` ?

JavaScript a toujours la notion d'au moins un environnement que nous ne contrôlons pas: un environnement global dans lequel de nombreuses choses utiles sont liées, telles que des bibliothèques remplies de fonctions standard. Ainsi, lorsque vous appelez `(function (x) {return x}) (1)` dans le REPL, son environnement complet va ressembler à ceci : `{x: 1, '..': `*environnement global*`}`.

Parfois, les programmeurs souhaitent éviter cela. Si vous ne souhaitez pas que votre code fonctionne directement dans l'environnement global, que pouvez-vous faire? Créer un environnement pour eux, bien sûr. Beaucoup de programmeurs choisissent d'écrire chaque fichier JavaScript comme ceci :

    // top of the file
    (function () {

      // ... lots of JavaScript ...

    })();
    // bottom of the file

Cela a pour effet d'insérer un nouvel environnement vide entre l'environnement global et vos propres fonctions: `{x: 1, '..': {'..': `*environnement global*`}}`. Comme nous le verrons plus tard, cela permet d'éviter que les programmeurs modifient accidentellement l'état global partagé par le code dans chaque fichier lorsqu'ils utilisent correctement le [mot-clé var](#var).
