# Elm en tant que support

React et Redux permettent d'obtenir une première version d'un frontend permettant d'écrire une webapp réactive sans rechargement de page, et sans avoir à gérer manuellement le DOM. Toutefois, cela ne permet pas de se passer de JavaScript intégralement. Pour résoudre les problèmes qui y sont liés, il est nécessaire de changer de langage. Or, Redux est fortement inspiré de Elm, un langage de programmation fonctionnel permettant l'écriture de webapp riche en navigateur.

## Un langage fonctionnel pour le web

Elm est un langage à la base fortement inspiré de Haskell. Il est ainsi fortement typé, purement fonctionnel, mais à la différence de Haskell, dispose d'une évaluation stricte et non paresseuse. Elm est un langage visant à être mainstream. Pour cela, il vise à fournir le meilleur environnement de développement pour les débutants comme pour les confirmés, et cherche au maximum à simplifier le langage. Ainsi, les premières versions de Elm en faisait un langage destiné à faire du FRP, ou Functional Reactive Programming. Ce paradigme étant jugé trop compliqué et inutile pour la majorité des utilisateurs, la version 0.17 abandonna les signaux au profit de l'architecture Elm \(The Elm Architecture\). Pour la même raison, Elm n'inclue pas de typeclass ou de functors comme on peut trouver dans la majorité des langages fonctionnels purs. Elm se concentre sur le développement d'applications web et sur la simplicité d'utilisation.

Elm compile directement vers JavaScript, mais à la direction d'un langage comme PureScript, Elm embarque un runtime pour s'exécuter. Il est de plus compliqué de faire de l'intéropérabilité avec JavaScript : il faut obligatoirement passer par des ports, permettant d'assurer le typage de toutes les données entrantes et sortantes de Elm.

## Une architecture pour le développement de SPA

Elm se distingue de nombreux langages de programmation par la volonté forte de son créateur de ne pas en faire un langage généraliste. En effet, Elm a pour vocation de devenir le langage de choix pour la création de webapp. L'intégralité de l'écosytème de Elm tourne autour de cela, et autour de son design pattern principal : l'architecture Elm, ou The Elm Architecture \(TEA\).

The Elm Architecture reprend exactement les mêmes caractéristiques que Redux : toutes les applications Elm se composent d'un modèle des données, d'un mécanisme d'update, et d'un mécanisme de rendu de HTML en fonction du modèle de données. Dans son exemple le plus simple, un programme Elm reprenant à son tour le compteur implémenté en React [se code ainsi](https://ellie-app.com/4cZmysTnCFPa1/0) :

```elm
import Html exposing (Html, button, div, text)
import Html.Events exposing (onClick)

main =
  Html.beginnerProgram
    { model = model
    , view = view
    , update = update
    }

-- Model
type alias Model = Int

model : Model
model =
  0

-- Messages
type Msg
  = Increment
  | Decrement

-- Update
update : Msg -> Model -> Model
update msg model =
  case msg of
    Increment ->
      model + 1

    Decrement ->
      model - 1

-- View
view : Model -> Html Msg
view model =
  div []
    [ button 
      [ onClick Decrement ] 
      [ text "-" ]
    , div 
      [] 
      [ text (toString model) ]
    , button 
      [ onClick Increment ] 
      [ text "+" ]
    ]
```

On retrouve les 3 parties de l'architecture : le modèle de données \(`Model`\), le mécanisme d'update \(`update` et `Msg`\) ainsi que le mécanisme de rendu \(`view`\).

De la même manière que React et Redux, la première exécution du programme déclenche la fonction de vue avec le model fournit en paramètre, et génère un Virtual DOM, qui génère ensuite un DOM sur le navigateur de l'utilisateur. Chaque élément de l'interface est géré directement par l'architecture, et les messages envoyés en JavaScript sont interceptés par le runtime pour envoyer les messages spécifiques à Elm et pouvoir exécuter la fonction d'update, celle-ci mettant à jour le Model, ce qui déclenche alors la fonction de rendu. De la même manière que React, seul le strict minimum est alors rendu dans le navigateur après un diff du Virtual DOM entre le précédent et le nouveau DOM généré.

En interceptant également l'URL du navigateur de l'utilisateur, il devient également possible d'effectuer du routing, et d'afficher des vues différentes selon la page sur laquelle veut se rendre l'utilisateur. Le modèle de la Single Page Application est alors respecté, et toutes les interactions avec un serveur se fait sans recharger de page, mais en générant des requêtes AJAX, et en les traitant comme un message dans la fonction d'update lors de leur retour.

Cela permet de définir une architecture fonctionnelle pure, tout en communiquant avec l'extérieur. Par ailleurs, on peut observer que le flux de données devient uni-directionnel \(_one-way data-flow_\). Il devient impossible d'avoir à faire à des effets de bords inattendus. Effectivement, chaque effet de bord passe obligatoirement par la fonction d'update, et est géré directement par le runtime. De plus, comme cela génère un nouvel état de l'application, il est possible d'explorer les états de l'application dans le temps en stockant ces états au fil du temps avec leur message.

Cette architecture permet de résoudre définitivement les problèmes posés par HTML : Elm se charge de la génération de HTML à l'aide des fonctions de génération \(ce qui évite d'avoir à gérer la structure de HTML, qui n'est pas toujours fiable\). Par ailleurs, la logique liée à l'interface est également directement gérée en Elm, sans besoin d'effectuer de modifications de DOM manuellement. Cela permet donc déjà, dans un premier temps, de s'affranchir de HTML et JavaScript. En revanche, CSS reste toujours indispensable pour structurer le frontend, et le styliser.

