# Du style sans CSS

Elm apporte une réponse convaincante pour se passer de HTML et JavaScript, toutefois CSS reste obligatoire, malgré l'architecture Elm. Pour résoudre les problèmes subsitant dans le web, il faut donc supprimer également CSS.

## HTML inline styling

### Les avantages de l'inline styling

Avec les mouvements récents dans le monde CSS, l'état de l'art actuel consiste à utiliser de plus en plus CSS comme un constructeur de styles de base, que l'on assemble dans le HTML. Dans une architecture comme Elm, il est possible d'utiliser le CSS de la même façon. Ainsi, un framework comme elm-css se concentre sur l'écriture de CSS en Elm, mais continue de lier des feuilles de style au niveau du HTML lui-même. Or, lorsque l'on utilise CSS avec des concepts comme BEM ou Atomic CSS, on se rapproche de plus en plus des styles en ligne \(inline styling\) dans le HTML. En effet, dans HTML, il est possible d'avoir un attribut `style` , contenant du CSS. Ce dernier n'agit que sur l'élément en question, et n'a pas d'influence sur le reste de la page, comme peut le faire une modification de la feuille de style. Cela supprime également les problèmes de namespace : les styles n'existent plus dans le namespace global, et il n'y a plus de risques de collision de noms. On supprime également les dépendances avec les autres éléments : puisque chaque élément déclare son propre style, chaque élément ne fait qu'hériter des propriétés parentes, puis redéfinit les siennes. Pour les mêmes raisons, le cascading n'a plus lieu d'être : chaque élément définit son style, il n'y a donc plus de chaînes de cascades de styles.

### Son implémentation en Elm

Pour implémenter directement l'inline styling, une bibliothèque a été écrite : [Elegant](https://github.com/elm-bodybuilder/elegant). Cette bibliothèque tente de résoudre le problème d'inline styling en permettant d'écrire chaque style pour chaque élément de manière fonctionnelle : le style devient une suite de fonctions de premier niveau, composable entre elles. Cela permet de traiter le style non plus comme une donnée statique, mais fonctionnelle, et réutilisable simplement.

```
button otherStyle =
  Html.button
    [ Elegant.style
      [ Elegant.displayBlock
      , Elegant.color (Color.rgb 255 255 255) 
      , otherStyle
      ]
    ]
    [ -- Html in there 
    ]
```



## Les problèmes insolubles

### Le hover et les pseudo classes

* Comportement
* Impossible avec l'inline styling
* Nécessite des classes

### Les media query

* Nécessite des classes
* Change la structure de la page
* Réagit selon la taille de la page
* Ne déclenche pas d'évènement

### Les essais infructeux

* Le hover grâce à des ids
  * Perte de l'avantage de ne pas écrire de global
  * Impossible de générer des ids
  * Lourdeur de l'écriture
  * Nécessite de gérer des évènements non nécessaires
* Les media queries avec une souscription à la taille de la page



