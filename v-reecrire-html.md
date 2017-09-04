# Réécrire HTML

Dans cette volonté de réécrire et redesigner les différents besoins du web, la reflexion autour de HTML nous a permis de mettre à jour différents besoins dans un package HTML au sein de Elm.

## Le typage d'HTML

HTML a un standard dense et compliqué, et les attributs que l'on peut lier sur les éléments sont différents selon les éléments. Les éléments eux-mêmes peuvent ne pas pouvoir s'agencer les uns dans les autres. Le typage de HTML permet de résoudre ce problème : en typant fortement toutes les balises et leurs attributs, cela permet d'éviter à la compilation d'obtenir un code inutilisable. Par exemple, mettre un élément dans un input n'est pas autorisé :

```html
<input type=text>
  Not allowed!!!
</input>
```

En utilisant le typage, et en forçant l'usage de fonctions n'acceptant pas ces éléments \(par exemple : `input : List (Attributes) -> Html msg`\), on peut s'assurer de ne jamais rencontrer ce problème. La compilation assure elle-même que le code répond au standard et garanti la justesse du programme.

Cela garanti également de ne plus avoir de problèmes de comportement, puisque les comportements impossible \(mettre un lien dans un lien par exemple\) sont interdits.

## HTML et CSS comme composante de base

* HTML comme building blocks
* Incorporer CSS
* Générer des classes atomiques
* Lier les styles aux éléments
* Modification du DOM au besoin



