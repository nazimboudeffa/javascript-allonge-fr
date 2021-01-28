## References et Objets {#objects}

JavaScript fournit également des objets. Le mot "objet" est chargé dans les cercles de programmation, en raison de l'utilisation répandue du terme "programmation orientée objet" qui a été inventé par Alan Kay mais qui a depuis pris un sens très large pour de nombreuses personnes.

En JavaScript, un objet [^pojo] est une carte qui va des noms aux valeurs, un peu comme un environnement. La syntaxe la plus courante pour créer un objet est simple :

[^pojo] : La tradition voudrait que nous appelions les objets qui ne contiennent aucune fonction "POJOs", ce qui signifie Plain Old JavaScript Objects.

    { year: 2012, month: 6, day: 14 }

Deux objets créés de cette façon ont des identités différentes, tout comme les tableaux :

    { year: 2012, month: 6, day: 14 } === { year: 2012, month: 6, day: 14 }
      //=> false

Les objets utilisent `[]` pour accéder aux valeurs par leur nom, en utilisant une chaîne de caractères :

    { year: 2012, month: 6, day: 14 }['day']
      //=> 14

Les valeurs contenues dans un objet fonctionnent de la même manière que les valeurs contenues dans un tableau :

    var unique = fonction () {
                    fonction de retour () {}
                  },
        x = unique(),
        y = unique(),
        z = unique(),
        o = { a : x, b : y, c : z } ;

    o['a'] === x && o['b'] === y && o['c'] === z
      //=> true

Les noms n'ont pas besoin d'être des chaînes alphanumériques. Pour toute autre chose, mettez l'étiquette entre guillemets :

    { 'first name': 'reginald', 'last name': 'lewis' }['first name']
      //=> 'reginald'

Si le nom est une chaîne alphanumérique conforme aux mêmes règles que les noms de variables, il existe une syntaxe simplifiée pour accéder aux valeurs :

    { year: 2012, month: 6, day: 14 }['day'] ===
        { year: 2012, month: 6, day: 14 }.day
      //=> true

Tous les conteneurs peuvent contenir n'importe quelle valeur, y compris des fonctions ou d'autres conteneurs :

    var Mathematics = {
      abs: function (a) {
             return a < 0 ? -a : a
           }
    };

    Mathematics.abs(-5)
      //=> 5

C'est drôle qu'on parle de "mathématiques". Si vous vous souvenez bien, JavaScript fournit un environnement global qui contient quelques objets existants qui ont des fonctions pratiques que vous pouvez utiliser. L'un d'entre eux s'appelle "Math", et il contient des fonctions pour "abs", "max", "min", et bien d'autres. Comme il est toujours disponible, vous pouvez l'utiliser dans n'importe quel environnement à condition de ne pas faire d'ombre à `Math`.

    Math.abs(-5)
      //=> 5

