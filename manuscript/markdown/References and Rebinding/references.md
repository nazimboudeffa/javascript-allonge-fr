### une simple question

une question simple
Considérez ce code :

    var x = "June 14, 1962",
        y = x ;

    x === y
      //=> true
C'est une évidence, car nous savons que les chaînes de caractères sont un type de valeur, donc quelle que soit l'expression utilisée pour obtenir la valeur "14 juin 1962", vous obtiendrez une chaîne de caractères ayant exactement la même identité.

Mais qu'en est-il de ce code ?

    var x = [2012, 6, 14],
        y = x ;

    x === y
      //=> true
C'est également vrai, même si nous savons que chaque fois que nous évaluons une expression telle que [2012, 6, 14], nous obtenons un nouvel éventail avec une nouvelle identité. Alors que se passe-t-il dans nos environnements ?

arguments et références
Dans notre discussion sur les fermetures, nous avons dit que les environnements lient des valeurs (comme [2012, 6, 14]) à des noms (comme x et y), et que lorsque nous utilisons ces noms comme des expressions, le nom est évalué comme la valeur.

Cela signifie que lorsque nous écrivons quelque chose comme y = x, le nom x est recherché dans l'environnement actuel, et sa valeur est un tableau spécifique qui a été créé lorsque l'expression [2012, 6, 14] a été évaluée pour la première fois. Nous lions ensuite cette même valeur au nom y dans un nouvel environnement, et donc x et y sont tous deux liés à la même valeur, qui est identique à elle-même.

La même chose se produit avec la liaison d'une variable par un moyen plus conventionnel d'appliquer une fonction à des arguments :

    var x = [2012, 6, 14] ;

(fonction (y) [2012, 6, 14]) ; (fonction (y) [2012, 6, 14]).
  return x === y
})(x)
  //=> true
x et y sont tous deux liés à la même matrice, et non à deux matrices différentes qui se ressemblent à nos yeux.
