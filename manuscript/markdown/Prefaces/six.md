## JavaScript Allongé, la "Six" Edition

Ceci est la version originale de JavaScript Allongé. Il a été écrit pour ECMAScript-5. Le thème général du livre et son approche de la programmation sont aussi valables qu’aujourd’hui quand ECMAScript-5 était la norme pour JavaScript, mais les détails de la meilleure façon de mettre en œuvre ces idées ont changé.

Par example, en ECMAScript-5, on écrit :

    function maybe (fn) {
      return function () {
        var i;

        if (arguments.length === 0) {
          return
        }
        else {
          for (i = 0; i < arguments.length; ++i) {
            if (arguments[i] == null) return
          }
          return fn.apply(this, arguments)
        }
      }
    }

Mais en ECMAScript-2015, on écrit :

    const maybe = (fn) =>
      function (...args) {
        if (args.length === 0) {
          return
        }
        else {
          for (let arg in args) {
            if (arg == null) return;
          }
          return fn.apply(this, args)
        }
      }

Parmi les autres modifications, citons l'introduction du mot-clé `class`, qui suscite un intérêt accru pour l'utilisation d'objets, de prototypes et de fonctions.

Pour cette raison, ce manuscrit original a été retiré et une édition sensiblement mise à jour, [JavaScript Allongé, l'édition "Six"] [j6] a été écrite. S'il vous plaît profiter de cette copie, mais assurez-vous de lire la dernière édition.

[j6]: https://leanpub.com/javascriptallongesix
