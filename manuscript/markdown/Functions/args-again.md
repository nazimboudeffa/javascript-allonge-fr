## I'd Like to Have Some Arguments. Again. {#arguments-again}

Comme nous l'avons vu, lorsqu'une fonction est appliquée à des arguments (ou "appelés"), JavaScript lie les valeurs des arguments aux noms d'arguments de la fonction dans un environnement créé pour l'exécution de la fonction. Ce dont nous n’avons pas parlé, c’est que JavaScript lie également certains noms "magiques" en plus de ceux que vous mettez dans la liste des arguments.

Vous ne devriez jamais essayer de définir vos propres liaisons avec ces noms. Considérez-les en lecture seule à tout moment. Le premier s'appelle `this` et est lié à quelque chose appelé [context](#context) de la fonction. Nous allons explorer cela lorsque nous commencerons à discuter d'objets et de classes. La seconde est très intéressante, elle s'appelle `arguments`, et le plus intéressant, c'est qu'elle contient une liste d'arguments passés à la fonction :

    function plus (a, b) {
      return arguments[0] + arguments[1]
    }

    plus(2,3)
      //=> 5

Bien que `arguments` ressemble à un tableau, ce n'est pas un tableau: [^pojo] C'est plutôt un objet [^pojo] qui lie certaines valeurs à des propriétés avec des noms qui ressemblent à des entiers commençant par zéro :

    function args (a, b) {
      return arguments
    }

    args(2,3)
      //=> { '0': 2, '1': 3 }

`arguments` contient toujours tous les arguments passés à une fonction, quel que soit le nombre déclaré. Par conséquent, nous pouvons écrire `plus` comme ceci:

    function plus () {
      return arguments[0] + arguments[1]
    }

    plus(2,3)
      //=> 5

Lors de la discussion d'objets, nous discuterons des propriétés plus en profondeur. Voici quelque chose d'intéressant à propos de `arguments`:

    function howMany () {
      return arguments['length']
    }

    howMany()
      //=> 0

    howMany('hello')
      //=> 1

    howMany('sharks', 'are', 'apex', 'predators')
      //=> 4

L'utilisation la plus courante de la liaison `arguments` est de créer des fonctions pouvant prendre un nombre variable d'arguments. Nous verrons cela utilisé dans de nombreuses recettes, en commençant par [partial application](#simple-partial) and [ellipses](#ellipses).

[^pojo]: Nous verrons plus tard les tableaux [arrays](#arrays) etd [plain old javascript objects](#objects).
