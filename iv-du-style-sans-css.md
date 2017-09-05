# Du style sans CSS

Elm apporte une réponse convaincante pour se passer de HTML et JavaScript, toutefois CSS reste obligatoire, malgré l'architecture Elm. Pour résoudre les problèmes subsitants dans le web, il faut donc supprimer également CSS.

## HTML inline styling

### Les avantages de l'inline styling

Avec les mouvements récents dans le monde CSS, l'état de l'art actuel consiste à utiliser de plus en plus CSS comme un constructeur de styles de base, que l'on assemble dans le HTML. Dans une architecture comme Elm, il est possible d'utiliser le CSS de la même façon. Ainsi, un framework comme [elm-css](https://github.com/rtfeldman/elm-css) se concentre sur l'écriture de CSS en Elm, mais continue de lier des feuilles de style au niveau du HTML lui-même. Or, lorsque l'on utilise CSS avec des concepts comme BEM ou Atomic CSS, on se rapproche de plus en plus des styles en ligne \(inline styling\) dans le HTML. En effet, dans HTML, il est possible d'avoir un attribut `style` , contenant du CSS. Ce dernier n'agit que sur l'élément en question, et n'a pas d'influence sur le reste de la page, comme peut le faire une modification de la feuille de style. Cela supprime également les problèmes de namespace : les styles n'existent plus dans le namespace global, et il n'y a plus de risques de collision de noms. On supprime également les dépendances avec les autres éléments : puisque chaque élément déclare son propre style, chaque élément ne fait qu'hériter des propriétés parentes, puis redéfinit les siennes. Pour les mêmes raisons, le cascading n'a plus lieu d'être : chaque élément définit son style, il n'y a donc plus de chaînes de cascades de styles.

```html
<!-- ->
<div style="color: red;">
  "I'm a stegosaurus!"
</div>
```

### Son implémentation en Elm

Pour implémenter directement l'inline styling, une bibliothèque a été écrite : [Elegant](https://github.com/elm-bodybuilder/elegant). Cette bibliothèque tente de résoudre le problème d'inline styling en permettant d'écrire chaque style pour chaque élément de manière fonctionnelle : le style devient une donnée de la structure, modifiée par une suite de fonctions générant un nouveau style. Ces fonctions sont de plus composables entre elles. Cela permet de traiter le style non plus comme une donnée statique, mais fonctionnelle, et réutilisable simplement.

Un exemple simple de style inline s'écrit ainsi :

```elm
button : (Style -> Style) -> Html msg
button otherStyleModifier =
  Html.button
    [ Elegant.style
      [ Elegant.displayBlock
      , Elegant.textColor (Color.rgb 255 255 255) 
      , otherStyleModifier
      ]
    ]
    [ Html.text "Button label"
    ]
```

Dans cette exemple, le HTML sous-jacent sera compilé sous la forme :

```html
<button style="display: block; color: rgb(255, 255, 255);">
  "Button label"
</button>
```

La fonction `Elegant.style` prend en paramètre une liste de fonctions modifiant le style. `Elegant.style` se type : `List (Style -> Style)`. Cela illustre bien cette nouvelle manière de considérer le style : il ne s'agit plus de données statiques fixées au lancement du programme, mais d'une structure de données pouvant se modifier au fur et à mesure de l'exécution du programme. Un style n'est donc plus unique, mais peut évoluer durant l'exécution d'un programme. Chaque fonction de la liste de modificateurs modifie alors ce style, qui sera ensuite compilé en une chaîne de caractères pouvant être utilisé directement dans les nœuds HTML.

Il est donc alors possible de définir des styles personnalisés. Puisqu'il s'agit de fonctions, celles-ci peuvent se composer et prendre des paramètres différents. Par exemple, il est possible de définir un style de base pour tous les éléments, qu'il est possible de réutiliser facilement :

```elm
baseStyle : Color -> Color -> (Style -> Style)
baseStyle primaryColor secondColor =
  Elegant.backgroundColor primaryColor 
    >> Elegant.textColor secondColor
```

Que l'on peut alors réutiliser et composer de la même manière :

```elm
example : Color -> Color -> Maybe Color -> (Style -> Style) -> Html msg
example primary second third otherStyleModifier =
  Html.p
    [ Elegant.style 
      [ (baseStyle primary second) >> backgroundColor (Maybe.withDefault primary third)
      ,  otherStyleModifier
      ]
    ]
    [ Html.text "Example" ]
```

Malheureusement, bien que l'inlining de styles et la transformation du style en valeur fonctionnelle permettent de résoudre beaucoup de problèmes de CSS, cela ne permet pas de retrouver l'intégralité des fonctionnalités de CSS.

## Les problèmes insolubles

### Le hover et les pseudo classes

Le comportement le plus évident utilisé en CSS et impossible à utiliser en inline styling est le hover, ou le comportement au survol — et par extension tous les pseudo-sélecteur et pseudo-classes. Pare exemple, le hover est défini en CSS avec le pseudo-sélecteur `:hover` :

```css
.red-hover:hover {
  color: red;
}
```

Cette pseudo-sélection agit sur des éléments qui n'ont pas d'identifiant précis dans le DOM : par exemple, les éléments hover n'ont de sens que lorsqu'un élément est survolé avec la souris de l'utilisateur. Il est donc impossible d'utiliser l'inline styling avec ces éléments, puisqu'ils n'existent que sous condition. De la même manière, les sélecteurs `::before`, `::after`, ou `:active` n'ont en réalité pas beaucoup de sens lorsque l'on envisage CSS comme un outil de styles. Effectivement, ces pseudo-classes et sélecteurs ont en réalité un impact sur la structure de la page, ou bien sur la logique elle-même de la page : le survol d'un élément est un évènement, au même titre qu'un clic. Le changement de design de l'élément au survol devrait donc être géré par la logique applicative — et donc par Elm.

Il est ainsi facile de conceptualiser comment intégrer les ::before et ::after. Puisque Elm génère du HTML \(tout comme ces sélecteurs\), il « suffit » de générer du HTML supplémentaires, avec du style inline. Le problème du hover et du active est par contre plus compliqué : ces évènements ont lieu, mais la majeure partie du temps, l'utilisateur ne souhaite pas avoir à réimplémenter de comportement lors de l'utilisation d'un hover.

### Les media queries

Le deuxième comportement CSS impossible à retranscrire en inline styling est les media queries. Les media queries sont des sélecteurs CSS permettant d'adapter le CSS et de le changer dynamiquement à l'exécution selon la taille de l'écran de l'utilisateur du site. Pour qu'un site soit _Responsive_, il se doit d'utiliser des media queries pour pouvoir se réajuster à n'importe quel écran, de l'ordinateur au smartphone. Les media queries requièrent des classes pour être utilisées correctement, puisqu'elles « englobent » la définition de classe dans le CSS :

```css
@media (max-width: 400px) {
  .red-text {
    color: red;
  }
}

@media (min-width: 400px) and (max-width: 700px) {
  .red-text {
    color: blue;
  }
}
```

Ces deux précédents exemples illustrent le changement de couleur de texte de tous les éléments `red-text` lorsque la taille de l'écran change. Lorsque l'écran est de 0 à 400px, le texte est rouge, et jusqu'à 700px, il apparait bleu. Les classes changent littéralement de contenu, et aucun évènement n'est émis. Il s'agit d'une modification importante et pourtant silencieuse, impossible à retranscrire facilement, et impossible à retranscrire en Elm, du fait de l'absence d'évènements.

Des essais ont donc été effectués pour résoudre ces problèmes.

### Les essais infructeux

Le premier essai concerne le problème du hover. L'idée de base partait du principe que le changement de style au hover était un comportement et non un style. Pour cela, il fallait utiliser Elm pour pouvoir générer du style différent pour un élément selon si celui-ci était survolé ou non.

Une syntaxe lourde permettait de déclarer un élément comme apte à être survolé, et liait l'élément à un identifiant. Lors du survol, un évènement Elm était généré, et cet identifiant était stocké dans le modèle. Ensuite, les fonctions de rendu interrogeait le modèle, et si celui-ci contenait l'identifiant en question, le style au hover était appliqué.

Cela comportait de nombreux défauts, mais le défaut majeur était la nécessité d'écrire des identifiants pour chaque élément que l'on souhaitait survoler. En effet, générer et stocker des identifiants en Elm est extrêmement compliqué, voire impossible, du fait de l'absence de modèle impératif. Cela forçait également à gérer des évènements non nécessaires.

Quant aux media queries, le problème pouvait être réglé en souscrivant à la taille de la fenêtre. En Elm, il existe des mécanismes de souscriptions, permettant de faire remonter des informations lors d'évènements passifs extérieur au programme. La taille de la fenêtre est donc un évènement passif, passant dans l'update à chaque redimensionnement de la fenêtre. Toutefois, cela forçait l'utilisateur à construire son code en fonction de la taille de la fenêtre, et alourdissait encore le code. De plus cela ne résolvait pas le problème de l'orientation ou de l'appareil.

## Scinder le style de la structure

Après tous ces essais, il est apparu évident que notre problème résidait dans le fait que le HTML, tel que conçu, ne peut gérer la structure de la page seul, et CSS est indispensable pour pouvoir obtenir l'affichage désiré. Il a donc fallu repenser comment devait interagir HTML et CSS entre eux, et réattribuer à chacun leur rôle : la structure pour l'un, le style pour l'autre, et ne plus mélanger les deux.

