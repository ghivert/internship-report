# Réécrire HTML

Dans cette volonté de réécrire et redesigner les différents besoins du web, la réflexion autour de HTML nous a permis de mettre à jour différents besoins dans un package HTML au sein de Elm : il s'agit dans un premier temps de lier dans un package « bas-niveau » les différents besoins pour ensuite pouvoir les séparer de nouveau proprement.

## Le typage d'HTML

HTML a un standard dense et compliqué, et les attributs que l'on peut lier sur les éléments sont différents selon les éléments. Les éléments eux-mêmes peuvent ne pas s'agencer les uns dans les autres. Le typage de HTML permet de résoudre ce problème : en typant fortement toutes les balises et leurs attributs, cela permet d'éviter à la compilation d'obtenir un code inutilisable. Par exemple, mettre un élément dans un input n'est pas autorisé :

```html
<input type=text>
  Not allowed here!!!
</input>
```

En utilisant le typage, et en forçant l'usage de fonctions n'acceptant pas ces éléments \(par exemple : `input : List Attributes -> Html msg`, sans arguments de contenu garanti de ne pas en obtenir\), on peut s'assurer de ne jamais rencontrer ce problème. La compilation assure elle-même que le code répond au standard et garanti la justesse du programme.

Cela garanti également de ne plus avoir de problèmes de comportement, puisque les comportements impossible \(mettre un lien dans un lien par exemple\) sont interdits.

## HTML et CSS comme composantes de base

### Extraire le Style de la structure

Une fois le typage des différents éléments obtenus, nous avons du intégrer le style inline dans chaque élément défini. Pour cela, nous devions réfléchir à la meilleure manière d'intégrer CSS dans les éléments tout en conservant l'inline styling. Cela est rendu possible grâce à un parcours du Virtual DOM généré par Elm lors de chaque rendu pour en extraire le style, et l'isoler du reste de la structure. On obtient donc ainsi une séparation entre le HTML et le CSS. Un élément se définit donc par sa balise, ainsi que son style :

```elm
button =
  NewHtml.button
    [ Elegant.textColor Color.red ]
    [ Attributes.content "button" ]
    [ NewHtml.text "label" ]
```

Ce style, définit à l'aide de la librairie Elegant est donc la même structure de données que précédemment.

Dans sa version classique, chaque fonction HTML de Elm écrit directement dans le Virtual DOM sans se soucier de ses enfants, rendant impossible le parcours de l'arbre en Elm. Il nous a donc fallu passer par une structure intermédiaire : une structure d'arbre stockant les enfants. Ce faisant, nous pouvons récupérer les styles de chaque élément. Un parcours d'arbre permet alors de séparer style et structure comme indiqué :

```elm
type Html msg
  = Html
    { structure :
      { tag : String
      , attributes : List (Attribute msg)
      , content : List (Html msg)
      }
    , styles : List (Style -> Style)
    }

type alias StyledNode msg =
  { css : List (Style -> Style)
  , html : VirtualDom.Node msg
  }

getHtmlAndStyles : Html msg -> List (StyledNode msg) -> List (StyledNode msg)
getHtmlAndStyles html acc =
  extractCssAndHtml html :: acc

extractCssAndHtml : Html msg -> StyledNode msg
extractCSSAndHtml (Html html) =
  let children = List.foldr getHtmlAndStyles [] html.structure.content in
    { html =
      VirtualDom.node html.structure.tag
        (List.append
          (List.map Css.styleToClassName html.styles)
          html.structure.attributes)
        (List.map .html children)
    , css =
      List.append html.styles <|
        List.concatMap .css children
    }
```

Comme il est possible de le voir dans ce code, un parcours de l'arbre extrait le HTML, mais va en même temps compiler chaque composante du style en un nom de classe unique pour chaque nœud. Cela permet dans un second temps de générer des classes CSS atomiques.

### Générer le CSS correspondant au Style

Chaque style extrait est en réalité un assemblage de composantes variées : `display`, `position`, `border`, `opacity`, `color`, `background-color`, etc. Chacune de ces composantes peut se retrouver dans plusieurs éléments différents dans le DOM, et il est important de ne pas répéter des dizaines de fois la même classe CSS dans le code généré. Grâce à l'extraction de l'intégralité des styles des éléments, cela permet de s'assurer, pour chaque composante, que celle-ci n'a pas déjà été compilée. Cela permet alors de générer des classes atomiques, ne contenant qu'une instruction chacune. Chaque classe est unique, et partagée par tous les éléments en ayant besoin.

Ces classes étant déjà liés aux éléments de HTML, puisque ceux-ci ont été compilés auparavant, il est possible de générer ensuite tous les styles dans un nœud `<style>` directement au sein de la page, et de le placer comme premier nœud dans le Virtual DOM. En cas de modification des styles de la page, le Virtual DOM prend en charge le changement de CSS dans le nœud style, et le répercute dans le navigateur.

En procédant ainsi — en compilant du HTML — il est possible de profiter de toute la puissance et toute la flexibilité de CSS, sans ses défauts : il est possible d'utiliser les pseudo-sélecteurs, les media queries, et cela évite les différents problèmes de namespace et de redondance. Tout cela est pris en charge directement par l'algorithme, qui se charge d'éliminer les doublons.

```elm
view =
  NewHtml.div
    []
    []
    [ button Color.red "First button, red."
    , button Color.blue "Second button, blue."
    ]
    
button color textualContent =
  NewHtml.button
    [ Elegant.textColor color
    , Elegant.backgroundColor Color.black
    ]
    []
    [ Html.text textualContent ]
```

Compile vers :

```html
<div>
  <style>
    .text-color--red {
      color: rgb(255, 0, 0);
    }
    
    .text-color--blue {
      color: rgb(0, 0, 255);
    }
    
    .background-color--black {
      background-color: rgb(0, 0, 0);
    }
  </style>
  <div>
    <button class="text-color--red background-color--black">
      "First button, red."
    </button>
    <button class="text-color--blue background-color--black">
      "Second button, blue."
    </button>
  </div>
</div>
```



