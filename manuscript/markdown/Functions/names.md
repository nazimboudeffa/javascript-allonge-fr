## Naming Functions {#named-function-expressions}

Allons droit au but. Ce code ne nomme *pas* une fonction :

    var repeat = function (str) {
      return str + str
    };

Il ne nomme pas la fonction "répéter" pour la même raison que `var answer = 42` ne nomme pas le nombre `42`. Cet extrait de code lie une fonction anonyme à un nom dans un environnement, mais la fonction elle-même reste anonyme.

JavaScript *a* une syntaxe pour nommer une fonction, cela ressemble à ceci:

    var bindingName = function actualName () {
      //...
    };

Dans cette expression, `bindingName` est le nom de l'environnement, mais `actualName` est le nom réel de la fonction. Ceci est une expression de fonction *nommée*. Cela peut paraître déroutant, mais considérez les noms de liaison comme des propriétés de l’environnement, et non de la fonction elle-même. Et en effet le nom *est* une propriété:

    bindingName.name
      //=> 'actualName'

Dans ce livre, nous n'examinons pas les outils JavaScript, tels que les débogueurs intégrés aux navigateurs, mais nous noterons que lorsque vous parcourez des piles d'appels dans tous les outils modernes, le nom de la liaison de la fonction est ignoré, mais son nom actuel est affiché. utiles même s'ils n'obtiennent pas de liaison formelle, par exemple

    someBackboneView.on('click', function clickHandler () {
      //...
    });

Désormais, le nom réel de la fonction n'a aucun effet sur l'environnement dans lequel elle est utilisée. À blanc :

    var bindingName = function actualName () {
      //...
    };

    bindingName
      //=> [Function: actualName]

    actualName
      //=> ReferenceError: actualName is not defined

Donc, "actualName" n'est pas lié à l'environnement dans lequel nous utilisons l'expression de fonction nommée. Est-ce lié ailleurs ? Oui, ça l'est :

    var fn = function even (n) {
      if (n === 0) {
        return true
      }
      else return !even(n - 1)
    }

    fn(5)
      //=> false

    fn(2)
      //=> true

`even` est lié à la fonction elle-même, mais pas à l'extérieur. Ceci est utile pour créer des fonctions récursives.

### déclarations de fonction

Nous avons en fait enterré le fil. [^ Lede] Nommer des fonctions dans le but de déboguer n’est pas aussi important que ce que nous sommes sur le point de discuter. Il existe une autre syntaxe pour nommer et / ou définir une fonction. Cela s'appelle une *déclaration de fonction*, et cela ressemble à ceci :

    function someName () {
      // ...
    }

Cela se comporte un *peu* comme :

    var someName = function someName () {
      // ...
    }

En cela, il lie un nom dans l'environnement à une fonction nommée. Cependant, considérons ce morceau de code :

    (function () {
      return someName;

      var someName = function someName () {
        // ...
      }
    })()
      //=> undefined  

C’est ce à quoi nous nous attendons compte tenu de ce que nous avons appris sur [var](# var) : Bien que `someName` soit déclaré plus tard dans la fonction, JavaScript se comporte comme si vous aviez écrit :

    (function () {
      var someName;

      return someName;

      someName = function someName () {
        // ...
      }
    })()

Qu'en est-il d'une déclaration de fonction sans `var` ?

    (function () {
      return someName;

      function someName () {
        // ...
      }
    })()
      //=> [Function: someName]

Aha ! Cela fonctionne différemment, comme si vous aviez écrit :

    (function () {
      var someName = function someName () {
        // ...
      }
      return someName;
    })()

Cette différence est intentionnelle de la part de la conception de JavaScript afin de faciliter un certain style de programmation dans lequel vous mettez la logique principale à l’avant et les "fonctions d’aide" en bas. Il n'est pas nécessaire de déclarer les fonctions de cette manière en JavaScript, mais il est essentiel de comprendre la syntaxe et son comportement (en particulier sa différence avec `var`) pour travailler avec le code de production.

### function declaration caveats[^caveats]

Les déclarations de fonction ne sont formellement supposées être faites qu'à ce que nous pourrions appeler le "niveau supérieur" d'une fonction. Bien que certains environnements JavaScript le permettent, cet exemple est techniquement illégal et constitue une mauvaise idée:

    // function declarations should not happen inside of
    // a block and/or be conditionally executed
    if (frobbishes.arePizzled()) {
      function complainToFactory () {
        // ...
      }
    }

Le gros problème avec les expressions telles que celle-ci est qu'elles peuvent très bien fonctionner dans votre environnement de test mais fonctionner différemment en production. Ou bien, cela peut fonctionner aujourd'hui et d'une manière différente lorsque le moteur JavaScript est mis à jour, par exemple avec une nouvelle optimisation.

Une autre mise en garde est qu'une déclaration de fonction ne peut pas exister à l'intérieur de *toute* expression, sinon c'est une expression de fonction. Donc, ceci est une déclaration de fonction:

    function trueDat () { return true }

Mais ce n'est pas :

    (function trueDat () { return true })

Les parenthèses en font une expression.

[^lede]: Un paragraphe de tête (ou lede) dans la littérature fait référence au paragraphe d'introduction d'un article, d'un essai, d'un reportage ou d'un chapitre de livre. En journalisme, le fait de ne pas mentionner les éléments les plus importants, les plus intéressants ou les plus captivants d'une histoire dans le premier paragraphe est parfois appelé "enterrer la vérité".

[^caveats]: Un certain nombre des mises en garde évoquées ici ont été décrites dans l'excellent article de Jyrly Zaytsev [Named function expressions demystified](http://kangax.github.com/nfe/).
