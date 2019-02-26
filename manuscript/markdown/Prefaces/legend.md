## Legend

Un texte de type monospaced tel que `this` dans le texte représente du code en cours de discussion. Certains codes monospaces dans ses propres lignes représentent également le code en cours de discussion :

    this.async = do (async = undefined) ->

      async = (fn) ->
        (argv..., callback) ->
          callback(fn.apply(this, argv))

Parfois, il contiendra du code que vous pourrez saisir vous-même. Lorsque cela se produit, le résultat de la saisie de quelque chose sera souvent affiché avec `// =>`, comme ceci :

    2 + 2
      //=> 4

T> Un paragraphe marqué comme ceci est un "fait essentiel". Cela résume une idée sans rien ajouter de nouveau.

X> Un paragraphe marqué comme ceci est un exercice suggéré que vous devez effectuer vous-même.

A> Un paragraphe marqué comme ceci est un aparté. Il peut être ignoré en toute sécurité. Il contient whimsey et autres logorrhées doupleplusunserious qui ne seront *pas* testées.
