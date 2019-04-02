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

[^f2f]: Nous avons dit que vous ne pouvez pas appliquer une fonction à une expression. Vous * pouvez * appliquer une fonction à une ou plusieurs fonctions. Les fonctions sont des valeurs! Cela a des applications intéressantes, et elles seront explorées beaucoup plus à fond dans la suite [Functions That Are Applied to Functions](#consumers).

### variables and bindings

Right now everything looks simple and straightforward, and we can move on to talk about arguments in more detail. And we're going to work our way up from `function (diameter) { return diameter * 3.14159265 }` to functions like:

    function (x) { return (function (y) { return x }) }

A> `function (x) { return (function (y) { return x }) }` just looks crazy, as if we are learning English as a second language and the teacher promises us that soon we will be using words like *antidisestablishmentarianism*. Besides a desire to use long words to sound impressive, this is not going to seem attractive until we find ourselves wanting to discuss the role of the Church of England in 19th century British politics.
A>
A> But there's another reason for learning the word *antidisestablishmentarianism*: We might learn how prefixes and postfixes work in English grammar. It's the same thing with `function (x) { return (function (y) { return x }) }`. It has a certain important meaning in its own right, and it's also an excellent excuse to learn about functions that make functions, environments, variables, and more.

In order to talk about how this works, we should agree on a few terms (you may already know them, but let's check-in together and "synchronize our dictionaries"). The first `x`, the one in `function (x) ...`, is an *argument*. The `y` in `function (y) ...` is another argument. The second `x`, the one in `{ return x }`, is not an argument, *it's an expression referring to a variable*. Arguments and variables work the same way whether we're talking about `function (x) { return (function (y) { return x }) }`  or just plain `function (x) { return x }`.

Every time a function is invoked ("invoked" means "applied to zero or more arguments"), a new *environment* is created. An environment is a (possibly empty) dictionary that maps variables to values by name. The `x` in the expression that we call a "variable" is itself an expression that is evaluated by looking up the value in the environment.

How does the value get put in the environment? Well for arguments, that is very simple. When you apply the function to the arguments, an entry is placed in the dictionary for each argument. So when we write:

    (function (x) { return x })(2)
      //=> 2

What happens is this:

1. JavaScript parses this whole thing as an expression made up of several sub-expressions.
1. It then starts evaluating the expression, including evaluating sub-expressions
1. One sub-expression, `function (x) { return x }` evaluates to a function.
1. Another, `2`, evaluates to the number 2.
1. JavaScript now evaluates applying the function to the argument `2`. Here's where it gets interesting...
1. An environment is created.
1. The value '2' is bound to the name 'x' in the environment.
1. The expression 'x' (the right side of the function) is evaluated within the environment we just created.
1. The value of a variable when evaluated in an environment is the value bound to the variable's name in that environment, which is '2'
1. And that's our result.

When we talk about environments, we'll use an [unsurprising syntax][json] for showing their bindings: `{x: 2, ...}`. meaning, that the environment is a dictionary, and that the value `2` is bound to the name `x`, and that there might be other stuff in that dictionary we aren't discussing right now.

[json]: http://json.org/

### call by sharing

Earlier, we distinguished JavaScript's *value types* from its *reference types*. At that time, we looked at how JavaScript distinguishes objects that are identical from objects that are not. Now it is time to take another look at the distinction between value and reference types.

There is a property that JavaScript strictly maintains: When a value--any value--is passed as an argument to a function, the value bound in the function's environment must be identical to the original.

We said that JavaScript binds names to values, but we didn't say what it means to bind a name to a value. Now we can elaborate: When JavaScript binds a value-type to a name, it makes a copy of the value and places the copy in the environment. As you recall, value types like strings and numbers are identical to each other if they have the same content. So JavaScript can make as many copies of strings, numbers, or booleans as it wishes.

What about reference types? JavaScript does not place copies of reference values in any environment. JavaScript places *references* to reference types in environments, and when the value needs to be used, JavaScript uses the reference to obtain the original.

Because many references can share the same value, and because JavaScript passes references as arguments, JavaScript can be said to implement "call by sharing" semantics. Call by sharing is generally understood to be a specialization of call by value, and it explains why some values are known as value types and other values are known as reference types.

And with that, we're ready to look at *closures*. When we combine our knowledge of value types, reference types, arguments, and closures, we'll understand why this function always evaluates to `true` no matter what argument[^NaNPedantry] you apply it to:

    function (value) {
      return (function (copy) {
        return copy === value
      })(value)
    }

[^NaNPedantry]: Unless the argument is NaN, which isn't equal to anything, including itself
