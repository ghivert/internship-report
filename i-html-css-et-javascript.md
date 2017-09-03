# Spécificités des langages web et historique

La création d'un site web ou d'une webapp passe forcément par l'utilisation des trois langages historiques du web : HTML, CSS et JavaScript.

## HTML

HTML, ou HyperText Markup Language, a été créé en 1989. Le langage HTML a pour but de décrire la structure d'une page web. Le HTML se présente comme un arbre décrivant l'intégralité des noeuds sur une page, chaque noeud représentant un élément de l'interface. Bien qu'âgé, HTML dispose d'avantages certains, notamment sa robustesse. HTML est un langage éprouvé et mis en place dans l'intégralité des navigateurs aujourd'hui. Il s'agit d'un langage relativement simple à comprendre, et permet de représenter facilement ce que l'on souhaite. De nombreux langages et frameworks ont adoptées des technologies similaires : on peut ainsi noter qu'iOS ou Android utilise tous deux des fichiers XML pour décrire l'interface de leurs applications. JavaFX, maintenant livré dans toutes les distributions Java utilise également une syntaxe XML pour la description d'interface. JavaFX a d'ailleurs pour vocation de remplacer Swing dans Java, et d'abandonner à terme les interfaces décrites de manière impérative.

En effet, contrairement à des technologies comme GTK+ ou Qt, HTML est une technologie dite déclarative : on écrit d'avance le HTML, en fonction des données que l'on récupère, et l'interface est générée automatiquement. C'est ainsi que font les sites web « dynamiques » capable de générer un HTML personalisé en fonction de l'utilisateur. À l'inverse, à l'aide d'une technologie comme GTK+, les développeurs sont obligés d'écrire la logique derrière l'interface, d'instancier des objets ou des structures \(en C ou C++ par exemple\), et de s'assurer de l'absence de bugs. Il s'agit alors d'une manière impérative de créer des interfaces : on ne déclare plus une interface en fonction des données, on la construit. Il est intéressant de noter que la plupart des frameworks impératifs de création d'interface se dirigent également vers une création d'interface déclarative : GTK+ se dote par exemple de Glade, un outil de RAD générant des fichiers XML pour la création d'interface ; Qt utilise un format XML également pour stocker les interfaces désignés dans QtDesigner.

HTML a par contre des particularités pouvant être handicapants dans certains cas. HTML est ainsi un langage très verbeux, mais qui n'est surtout pas très fiable à utiliser : bien que similaire à XML dans sa structure, HTML autorise à fournir des fichiers à la structure inconsistante, le langage s'occupant des gérer les erreurs de l'utilisateur. Là où XML refuse le parsing si une balise ouvrante n'a pas de balise fermante correspondante, HTML peut dans certains cas fermer tout seul des balises, ou bien avoir un comportement imprévisible. Par ailleurs, le standard est compliqué, puiqu'il existe depuis plus de 30 ans : de nombreuses couches de compatibilité existent pour que tous les navigateurs puissent lire le HTML, depuis la première norme jusqu'à la dernière \(HTML5\). Cela inclut aussi que de nombreux attributs existent pour différents éléments, mais ne fonctionnent pas avec toutes les balises. Cela peut créer des situations compliqués, car HTML ne génère pas de message d'erreur. Il est donc possible de se retrouver avec des attributs sur des éléments n'ayant aucun impact, amenant de la confusion et de la maintenance supplémentaire. Pour s'assurer de la justesse d'un code, il faut donc le valider à l'aide de linter, capable de remonter les erreurs au développeur.

Enfin, HTML est par nature statique : il est impossible de le modifier à l'exécution sans passer par JavaScript. Cela pose des problèmes compliqués pour gérer de multiples pages HTML : il fallait forcément recharger la page avec un nouveau code HTML pour obtenir un affichage différent — gérer le DOM en JavaScript étant très compliqué à effectuer à cause des nombreux effets de bord induits.

## CSS

* Désigné au milieu des années 90
* Censé s'occuper du style
* Prend en charge structure et comportement
* Cascade et sélecteur compliqué
  * Pas de typage
  * Pas de message d'erreur
* Comportement imprévisible
* Nombreuses évolutions peu compatibles entre elles
* Absence de fonctionnalités importantes :
  * Namespace
  * Variables

## JavaScript

* Désigné en 1995
* Implémentations différentes selon le navigateur
  * Comportement non fiable
* Pas de message d'erreur
* Problème de portée lexicale



