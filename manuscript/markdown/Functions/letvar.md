## Parlons Var {#let}

Jusqu'à présent, tout ce que nous avons vraiment vu sont des *fonctions anonymes*, des fonctions qui n'ont pas de nom. C’est là que l’accent est mis sur les fonctions, méthodes et procédures de dénomination. Nommer des éléments est un élément essentiel de la programmation, mais nous savons tous comment argumenter.

Il existe d'autres moyens de nommer des éléments en JavaScript, mais avant d'en apprendre quelques-uns, voyons ce que nous faisons. Revenons sur un exemple très simple :

    function (diameter) {
      return diameter * 3.14159265
    }

Quel est ce numéro "3.14159265" ? [Pi], évidemment. Nous aimerions le nommer pour pouvoir écrire quelque chose comme :

    function (diameter) {
      return diameter * Pi
    }

Afin de binder `3.14159265` au nom de `Pi`, nous aurions besoin d'un paramètre de `Pi` appliqué à un argument de `3.14159265`. Si nous mettons notre expression de fonction entre parenthèses, nous pouvons l'appliquer à l'argument de `3.14159265` :

    (function (Pi) {
      return ????
    })(3.14159265)

Que mettons-nous dans notre nouvelle fonction qui lie `3.14159265` au nom `Pi` lors de l'évaluation ? Notre circonférence fonctionne, bien sûr :

[Pi]: https://en.wikipedia.org/wiki/Pi

    (function (Pi) {
      return function (diameter) {
        return diameter * Pi
      }
    })(3.14159265)

Cette expression, une fois évaluée, renvoie une fonction calculant les circonférences. Il diffère de l'original en ce qu'il nomme la constante `Pi`. Testons le:

    (function (Pi) {
      return function (diameter) {
        return diameter * Pi
      }
    })(3.14159265)(2)
      //=> 6.2831853

Ça marche ! Nous pouvons lier tout ce que nous voulons dans une expression en l'enveloppant dans une fonction qui est immédiatement appelée avec la valeur que nous voulons lier.

### a immédiatement appelé les expressions de fonction

Les programmeurs JavaScript utilisent régulièrement l’idée d’écrire une expression qui dénote une fonction, puis l’applique immédiatement aux arguments. Expliquant le motif, Ben Alman a inventé le terme [Immediately Invoked Function Expression][iife], souvent abrégé en "IIFE". Comme nous le verrons dans un instant, un IIFE n'a pas besoin de paramètres:

    (function () {
      // ... do something here...
    })();

When an IIFE binds values to names (as we did above with `Pi`), retro-grouch programmers often call it "let."[^let] And confusing the issue, upcoming versions of JavaScript have support for a `let` keyword that has a similar binding behaviour.

Quand un IIFE lie des valeurs à des noms (comme nous l’avons déjà fait avec `Pi`), les programmeurs de rétro-grouch l’appellent souvent "let" [^Let]. Et confondant le problème, les versions à venir de JavaScript ont un support pour un mot clé `let` qui a un comportement contraignant similaire.

### var {#var}

Utiliser un IIFE pour lier des noms fonctionne, mais seul un masochiste écrit des programmes de cette manière en JavaScript. Outre tous les caractères supplémentaires, il souffre d'un problème sémantique fondamental: il existe une grande distance visuelle entre le `Pi` et la valeur `3.14159265` que nous lui lions. Ils devraient être plus proches. Y a-t-il un autre moyen?

Oui.

Une autre façon d'écrire notre "circonférence" serait d'aller à `Pi` avec l'argument du diamètre, quelque chose comme ceci :

    function (diameter, Pi) {
      return diameter * Pi
    }

Et vous pourriez l'utiliser comme ceci :

    (function (diameter, Pi) {
      return diameter * Pi
    })(2, 3.14159265)
      //=> 6.2831853

Cela diffère de notre exemple dans un environnement plutôt que dans deux. Nous avons un lien dans l'environnement de notre argumentation régulière, et un autre de notre "constante". C'est plus efficace, et c'est *presque* toujours le cas : un moyen de lier `3.14159265` à un nom lisible.

JavaScript gives us a way to do that, the `var` keyword. We'll learn a lot more about `var` in future chapters, but here's the most important thing you can do with `var`:

JavaScript nous donne un moyen de faire cela, le mot clé `var`. Nous en apprendrons plus sur `var` dans les chapitres à venir, mais voici la chose la plus importante que vous puissiez faire avec `var` :

    function (diameter) {
      var Pi = 3.14159265;

      return diameter * Pi
    }

Le mot clé `var` introduit une ou plusieurs liaisons dans l'environnement de la fonction en cours. Cela fonctionne comme nous voulons :

    (function (diameter) {
      var Pi = 3.14159265;

      return diameter * Pi
    })(2)
      //=> 6.2831853

Vous pouvez lier n'importe quelle expression. Les fonctions sont des expressions, vous pouvez donc lier des fonctions d'assistance :

    function (d) {
      var calc = function (diameter) {
        var Pi = 3.14159265;

        return diameter * Pi
      };

      return "The circumference is " + calc(d)
    }

Notice `calc(d)`? This underscores what we've said: if you have an expression that evaluates to a function, you apply it with `()`. A name that's bound to a function is a valid expression evaluating to a function.[^namedfn]

Avis `calc(d)` ? Cela souligne ce que nous avons dit: si vous avez une expression qui donne une fonction, vous l’appliquez avec `()`. Un nom lié à une fonction est une expression valide évaluation d'une fonction [^namedfn]

[^namedfn]: Nous en sommes au deuxième chapitre et nous avons finalement nommé une fonction. Sheesh.

A> C'est incroyable de voir comment une idée aussi importante - nommer des fonctions - peut être expliquée *en passant* en quelques mots. JavaScript est vraiment, vraiment correct: fonctionne comme "entités de première classe". Les fonctions sont des valeurs pouvant être liées à d'autres valeurs, transmises sous forme d'arguments, renvoyées par d'autres fonctions, etc.

Vous pouvez lier plusieurs paires nom-valeur en les séparant par des virgules. Pour plus de lisibilité, la plupart des gens mettent une liaison par ligne :

    function (d) {
      var Pi   = 3.14159265,
          calc = function (diameter) {
            return diameter * Pi
          };

      return "The circumference is " + calc(d)
    }

Ces exemples utilisent le mot-clé `var` pour lier des noms dans le même environnement que notre fonction. Nous pouvons également créer une nouvelle portée à l'aide d'un IIFE si nous souhaitons lier certains noms dans une partie d'une fonction :

    function foobar () {

      // do something without foo or bar

      (function () {
        var foo = 'foo',
            bar = 'bar';

        // ... do something with foo and bar ...

      })();

      // do something else without foo or bar

    }

[^let]: To be pedantic, both main branches of Lisp today define a special construct called "let." One, Scheme, [uses `define-syntax` to rewrite `let` into an immediately invoked function expression that binds arguments to values](https://en.wikipedia.org/wiki/Scheme_(programming_language)#Minimalism) as shown above. The other, Common Lisp, leaves it up to implementations to decide how to implement `let`.

[^let]: Pour être pédant, les deux branches principales de Lisp définissent aujourd'hui une construction spéciale appelée "let". L'un, Scheme, [uses `define-syntax` to rewrite `let` into an immediately invoked function expression that binds arguments to values](https://en.wikipedia.org/wiki/Scheme_(programming_language)#Minimalism) comme indiqué au dessus de. L'autre, Common Lisp, laisse le soin à l'implémentation de décider comment implémenter `let`.

[iife]: http://www.benalman.com/news/2010/11/immediately-invoked-function-expression/
