## Un Mot Personel au Sujet des Recettes

Comme indiqué précédemment, *JavaScript Allongé* alterne entre les chapitres décrivant la sémantique des fonctions de JavaScript et les chapitres contenant les recettes permettant d'écrire des programmes avec des fonctions. Vous pouvez lire le livre dans l'ordre ou lire les chapitres expliquant d'abord JavaScript et revenir aux recettes plus tard.

Les recettes partagent un thème commun : elles sont issues d'un style de programmation inspiré par la création de petites fonctions qui composent les unes avec les autres. En utilisant ces recettes, vous apprendrez quand il convient d'écrire :

    return mapWith(maybe(getWith('name')))(customerList);

Au lieu de :

    return customerList.map(function (customer) {
      if (customer) {
        return customer.name
      }
    });

Ainsi que comment cela fonctionne et comment le refactoriser quand vous avez besoin. Ce style de programmation n’est guère la chose la plus courante en JavaScript. L’on peut donc affirmer que des recettes plus "pratiques" ou "banales" seraient utiles. Si vous ne lisez jamais d'autres livres sur JavaScript, si vous évitez les blogues et les vidéos sur JavaScript, si vous n'assistez pas à des ateliers ou des discussions sur JavaScript, alors je conviens que ce n'est pas un Seul Livre pour les Dominer Tous.

Mais étant donné qu’il existe d’autres ressources et que les programmeurs sont des créatures curieuses qui ont une soif inépuisable de croissance personnelle, nous avons choisi de vous proposer des recettes que vous ne trouverez probablement pas ailleurs dans cette concentration. Les recettes renforcent les leçons du livre sur les fonctions en JavaScript.

Vous trouverez toutes les recettes collectées en ligne sur [http://allong.es] (http://allong.es). Ils sont libres de partager sous la licence MIT.

[Reginald Braithwaite](http://braythwayt.com)  
reg@braythwayt.com  
@raganwald
