## Combinators and Function Decorators {#combinators}

### higher-order functions

Comme nous l'avons vu, les fonctions JavaScript prennent des valeurs comme arguments et renvoient des valeurs. Les fonctions JavaScript étant des valeurs, les fonctions JavaScript peuvent prendre des fonctions en tant qu'arguments, des fonctions de retour ou les deux. En règle générale, une fonction qui prend des fonctions en tant qu'arguments ou qui retourne une fonction (ou les deux) est appelée fonction "d'ordre supérieur".

Voici une fonction très simple d'ordre supérieur qui prend une fonction en argument :

    function repeat (num, fn) {
      var i, value;

      for (i = 1; i <= num; ++i)
        value = fn(i);

      return value;
    }

    repeat(3, function () {
      console.log('Hello')
    })
      //=>
        'Hello'
        'Hello'
        'Hello'
        undefined

Les fonctions d'ordre supérieur dominent *JavaScript Rallongé*. Mais avant de continuer, nous allons parler de certains types spécifiques de fonctions d’ordre supérieur.

### combinators

Le mot "combinateur" a une signification technique précise en mathématiques :

> "A combinator is a higher-order function that uses only function application and earlier defined combinators to define a result from its arguments."--[Wikipedia][combinators]

[combinators]: https://en.wikipedia.org/wiki/Combinatory_logic "Combinatory Logic"

Si nous apprenions la logique combinatoire, nous commencerions par les combinateurs les plus élémentaires tels que `S`, `K` et `I`, puis nous passerions à des combinateurs pratiques. Nous apprendrions que les combinateurs fondamentaux sont nommés d'après des oiseaux, à l'instar du célèbre livre de Raymond Smullyan [Mock a Mockingbird][mock].

[mock]: http://www.amazon.com/gp/product/B00A1P096Y/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=B00A1P096Y&linkCode=as2&tag=raganwald001-20

Dans ce livre, nous utiliserons une définition plus vague de "combinator:" Les fonctions pures d'ordre supérieur qui n'utilisent que des arguments et retournent une fonction. Nous ne serons pas stricts quant à l’utilisation de combinateurs précédemment définis dans leur construction.

Commençons par un combinateur utile: la plupart des programmeurs l'appellent *Compose*, même si les logiciens l'appellent le combinateur B ou "Bluebird". Voici l'implémentation typique de la programmation [^bluebird] :

    function compose (a, b) {
      return function (c) {
        return a(b(c))
      }
    }

Disons que nous avons :

    function addOne (number) {
      return number + 1
    }

    function doubleOf (number) {
      return number * 2
    }

Avec `compose`, vous écrivez n'importe où

    function doubleOfAddOne (number) {
      return doubleOf(addOne(number))
    }

Vous pouvez aussi écrire :

    var doubleOfAddOne = compose(doubleOf, addOne);

Ce n’est bien sûr qu’un exemple parmi d’autres. Vous en trouverez beaucoup plus en parcourant les recettes de ce livre. Bien que certains programmeurs estiment qu’il ne devrait exister qu’une façon de le faire, le fait d’avoir des combinateurs disponibles et d’écrire explicitement avec beaucoup de symboles et de mots-clés présente certains avantages lorsqu’il est utilisé judicieusement.

### une déclaration équilibrée sur les combinateurs

Le code qui utilise beaucoup de combinateurs a tendance à nommer les verbes et les adverbes (comme `doubleOf`,` addOne` et `compose`) tout en évitant les mots-clés de langue et les noms de noms (comme `number`). Ainsi, un point de vue est que les combinateurs sont utiles lorsque vous souhaitez mettre l'accent sur ce que vous faites et sur la manière dont cela s'articule, et un code plus explicite est utile lorsque vous souhaitez mettre l'accent sur ce avec quoi vous travaillez.

### function decorators {#decorators}

Un *décorateur de fonction* est une fonction d'ordre supérieur qui prend une fonction en tant qu'argument, retourne une autre fonction, et la fonction renvoyée est une variante de la fonction argument. Voici un exemple ridicule de décorateur:

    function not (fn) {
      return function (argument) {
        return !fn(argument)
      }
    }

Ainsi, au lieu d'écrire `!SomeFunction (42)`, vous pouvez écrire `not(someFunction)(42)`. Peu de progrès. Mais comme pour `compose`, si vous avez :

    function something (x) {
      return x != null
    }

Ensuite, vous pouvez écrire soit :

    function nothing (x) {
      return !something(x)
    }

Ou :

    var nothing = not(something);

`not` is a function decorator because it modifies a function while remaining strongly related to the original function's semantics. You'll see other function decorators in the recipes, like [once](#once), [mapWith](#mapWith), and [maybe](#maybe). Function decorators aren't strict about being pure functions, so there's more latitude for making decorators than combinators.

`not` est un décorateur de fonctions car il modifie une fonction tout en restant fortement lié à la sémantique de la fonction d'origine. Vous verrez d'autres décorateurs de fonctions dans les recettes, comme [une fois](#once), [mapWith](#mapWith) et [peut-être](#maybe). Les décorateurs de fonctions ne sont pas stricts sur le fait d'être des fonctions pures. Il est donc plus facile de faire des décorateurs que des combinateurs.

[^bluebird]: Comme nous le verrons plus tard, cette implémentation du B Combinator est correcte dans des langages tels que Scheme, mais pour une utilisation véritablement polyvalente en JavaScript, elle doit gérer correctement le [function context](#context).
