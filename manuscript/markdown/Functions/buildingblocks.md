## Building Blocks {#buildingblocks}

Lorsque vous examinez les fonctions au sein de fonctions en JavaScript, vous y trouverez un peu de "code spaghetti". La force de JavaScript est que vous pouvez faire n'importe quoi. La faiblesse est que vous allez le faire. Il y a des ifs, des fors, des retours, tout a été jeté ensemble. Bien que vous n’ayez pas besoin de vous limiter à un petit nombre de modèles simples, il peut être utile de comprendre ces modèles afin de structurer votre code autour de blocs de construction de base.

### composition

L'un des plus élémentaires de ces blocs de construction est la *composition* :

    function cookAndEat (food) {
      return eat(cook(food))
    }

C'est vraiment aussi simple que ça: chaque fois que vous chaînez deux ou plusieurs fonctions, vous les composez. Vous pouvez les composer avec du code JavaScript explicite comme nous venons de le faire. Vous pouvez également généraliser la composition avec le B Combinator ou "composer" comme nous l’avons vu dans [Combinators and Decorators](#combinators) :

    function compose (a, b) {
      return function (c) {
        return a(b(c))
      }
    }

    var cookAndEat = compose(eat, cook);

Si c'était tout ce qu'il y avait à faire, la composition n'aurait pas beaucoup d'importance. Mais comme beaucoup de modèles, l’utiliser quand il s’applique ne représente que 20% de l’avantage. Les 80% restants proviennent de l'organisation de votre code de sorte que vous puissiez l'utiliser: Fonctions d'écriture pouvant être composées de différentes manières.

Dans les recettes, nous allons examiner un décorateur appelé [une fois](#once): Cela garantit qu’une fonction ne peut être exécutée qu’une fois. Par la suite, cela ne fait rien. Once est utile pour éviter que certains effets secondaires ne se répètent. Nous allons aussi regarder [peut-être](#maybe): Cela garantit qu’une fonction ne fait rien si on ne lui donne rien (comme `null` ou` undefined`) en tant qu’argument.

Bien entendu, vous n'avez pas besoin d'utiliser de combinateur pour implémenter l'une ou l'autre de ces idées, vous pouvez utiliser les instructions if. Mais `une fois` et` peut-être` composent, vous pouvez donc les chaîner à votre guise:

    function actuallyTransfer(from, to, amount) {
      // do something
    }

    var invokeTransfer = once(maybe(actuallyTransfer(...)));

### application partielle

Une autre composante de base est *l'application partielle*. Lorsqu'une fonction prend plusieurs arguments, nous "appliquons" la fonction aux arguments en l'évaluant avec tous les arguments, produisant une valeur. Mais que se passe-t-il si nous ne fournissons que certains des arguments? Dans ce cas, nous ne pouvons pas obtenir la valeur finale, mais nous pouvons obtenir une fonction qui représente une * partie * de notre application.

Le code est plus facile que les mots pour cela. La bibliothèque [Underscore] fournit une fonction d'ordre supérieur appelée *map*. [^headache] Elle applique une autre fonction à chaque élément d'un tableau, comme ceci :

    _.map([1, 2, 3], function (n) { return n * n })
      //=> [1, 4, 9]

Ce code implémente une application partielle de la fonction map en appliquant la fonction `function (n) {return n * n}` comme second argument :

    function squareAll (array) {
      return _.map(array, function (n) { return n * n })
    }

La fonction résultante - `squareAll` - est toujours la fonction map, c'est juste que nous avons déjà appliqué l'un de ses deux arguments. `squareAll` est bien, mais pourquoi écrire une fonction à chaque fois que nous voulons appliquer partiellement une fonction à une carte ? Nous pouvons faire abstraction de ce niveau supérieur. `mapWith` prend n'importe quelle fonction en tant qu'argument et retourne une fonction de carte partiellement appliquée.

    function mapWith (fn) {
      return function (array) {
        return _.map(array, fn)
      }
    }

    var squareAll = mapWith(function (n) { return n * n });

    squareAll([1, 2, 3])
      //=> [1, 4, 9]

Nous discuterons de nouveau de mapWith dans [les recettes](#mapWith). La chose importante à voir est que l'application partielle est orthogonale à la composition, et qu'elles fonctionnent bien ensemble :

    var safeSquareAll = mapWith(maybe(function (n) { return n * n }));

    safeSquareAll([1, null, 2, 3])
      //=> [1, null, 4, 9]

Nous avons généralisé la composition avec le combinateur `compose`. L'application partielle dispose également d'un combinateur, que nous verrons dans la recette [partielle](#partial).

[^bind]: Modern JavaScript provides a limited form of partial application through the `Function.prototype.bind` method. This will be discussed in greater length when we look at function contexts.

[^headache]: les implémentations JavaScript modernes fournissent une méthode de mappage pour les tableaux, mais l'implémentation d'Underscore fonctionne également avec les navigateurs plus anciens si vous travaillez avec ce mal de tête.

[Underscore]: http://underscorejs.org
