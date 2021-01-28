## Arguments et tableaux {#arrays}

JavaScript fournit deux types différents de conteneurs pour les valeurs. Nous en avons déjà rencontré un, le tableau. Voyons comment il traite les valeurs et les identités. Pour commencer, nous allons apprendre à extraire une valeur d'un tableau. Nous commencerons par une fonction qui crée une nouvelle valeur avec une identité unique à chaque fois que nous l'appelons. Nous savons déjà que chaque fonction que nous créons est unique, c'est donc ce que nous utiliserons :

var unique = fonction () {
                fonction de retour () {}
              } ;

  unique()
    //=> [Fonction]
    
  unique() === unique()
    //=> false
Vérifions que ce que nous avons dit à propos des références s'applique aussi bien aux fonctions qu'aux tableaux :

  var x = unique(),
      y = x ;
      
  x === y
    //=> true
Ok. Alors, qu'en est-il des choses à l'intérieur des tableaux ? Nous savons comment créer un tableau avec quelque chose à l'intérieur :

  [unique()]
    //=> [ [Fonction] ]
C'est un tableau qui contient l'une de nos fonctions uniques. Comment en tirer quelque chose ?

  var a = ['hello'] ;
  
  a[0]
    //=> 'hello'
Cool, les tableaux fonctionnent beaucoup comme les tableaux dans d'autres langues et sont basés sur le zéro. Le problème avec cet exemple est que les chaînes de caractères sont des types de valeur en JavaScript, donc nous n'avons aucune idée si un [0] nous redonne toujours la même valeur comme pour la recherche d'un nom dans un environnement, ou s'il fait un peu de magie qui essaie de nous donner une nouvelle valeur.

Nous devons mettre un type de référence dans un tableau. Si nous obtenons la même chose en retour, nous savons que le tableau stocke une référence à ce que vous y mettez. Si vous obtenez quelque chose de différent en retour, vous savez que les tableaux stockent des copies de choses [^hunh].

[^hunh] : Les tableaux dans toutes les langues contemporaines stockent des références et non des copies, on peut donc nous pardonner de nous attendre à ce qu'ils fonctionnent de la même manière en JavaScript. Néanmoins, c'est un exercice utile pour tester les choses par soi-même.

Testons-le :

var unique = fonction () {
                fonction de retour () {}
              },
    x = unique(),
    a = [ x ] ;
    
a [0] === x
  //=> true
Si nous obtenons une valeur à partir d'un tableau en utilisant le suffixe [], c'est exactement la même valeur avec la même identité. Question : Cela s'applique-t-il aux autres emplacements du tableau ? Oui :

var unique = fonction () {
               fonction de retour () {}
             },
    x = unique(),
    y = unique(),
    z = unique(),
    a = [ x, y, z ] ;
    
a[0] === x && a[1] === y && a[2] === z
  //=> true
