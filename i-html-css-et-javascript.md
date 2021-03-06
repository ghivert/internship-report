# Spécificités des langages web historiques

La création d'un site web ou d'une webapp passe forcément par l'utilisation des trois langages historiques du web : HTML, CSS et JavaScript. Ceux-ci sont aujourd'hui les seuls langages pris en charge nativement sur tous les navigateurs, et le W3C s'assure de la normalisation des trois langages pour assurer une prise en charge identique quel que soient les navigateurs. La fin d'Internet Explorer garantit également la prise en charge correcte de ces normes quel que soit le navigateur utilisé pour consulter un site web. L'émancipation de ces langages passe donc dans un premier temps par leur compréhension pour comprendre par quoi les remplacer, et comment.

## HTML

HTML, ou HyperText Markup Language, a été créé en 1989. Le langage HTML a pour but de décrire la structure d'une page web. Le HTML se présente comme un arbre décrivant l'intégralité des noeuds sur une page, chaque noeud représentant un élément de l'interface. À l'exécution, le navigateur génère alors un DOM, ou Document Object Model, à partir du code représentant la structure du document. Il est alors possible d'interagir avec le DOM en question et de le modifier ou de l'explorer. Cela rend possible les interactions entre HTML et JavaScript, et permet d'avoir des modifications dynamiques de la page.

Bien qu'âgé, HTML dispose d'avantages certains, notamment sa robustesse. HTML est un langage éprouvé et mis en place dans l'intégralité des navigateurs aujourd'hui. Il s'agit d'un langage relativement simple à comprendre, et permet de représenter facilement ce que l'on souhaite. De nombreux langages et frameworks ont adoptées des technologies similaires : on peut ainsi noter qu'iOS ou Android utilise tous deux des fichiers XML pour décrire l'interface de leurs applications. JavaFX, maintenant livré dans toutes les distributions Java utilise également une syntaxe XML pour la description d'interface. JavaFX a d'ailleurs pour vocation de remplacer Swing dans Java, et d'amener à terme à l'abandon les interfaces décrites de manière impérative.

En effet, contrairement à des technologies comme GTK+ ou Qt, HTML est une technologie dite déclarative : on écrit d'avance le HTML, en fonction des données que l'on récupère, et l'interface est générée automatiquement. C'est ainsi que font les sites web « dynamiques » capable de générer un HTML personnalisé en fonction de l'utilisateur. À l'inverse, une technologie comme GTK+ force les développeurs à écrire la logique derrière l'interface, d'instancier des objets ou des structures \(en C ou C++ par exemple\), et de s'assurer eux-mêmes de l'absence de bugs. Il s'agit alors d'une manière impérative de créer des interfaces : on ne déclare plus une interface en fonction des données, on la construit. Il est intéressant de noter que la plupart des frameworks impératifs de création d'interface se dirigent également vers une création d'interface déclarative : GTK+ se dote par exemple de Glade, un outil de RAD, Rapid Application Design, générant des fichiers XML pour la création d'interface ; Qt utilise un format XML également pour stocker les interfaces désignés dans QtDesigner.

HTML a par contre des particularités pouvant être handicapantes dans certains cas. HTML est ainsi un langage très verbeux, mais qui n'est surtout pas très fiable à utiliser : bien que similaire à XML dans sa structure, HTML autorise à fournir des fichiers à la structure inconsistante, le langage s'occupant des gérer les erreurs du développeur. Là où XML refuse le parsing si une balise ouvrante n'a pas de balise fermante correspondante, HTML peut dans certains cas fermer seul des balises, ou bien avoir un comportement imprévisible. Par ailleurs, le standard est compliqué. Du fait de ses 30 ans d'existence, de nombreuses couches de compatibilité existent pour que tous les navigateurs puissent lire le HTML, depuis la première norme jusqu'à la dernière \(HTML5\). Il est également possible de lier des attributs directement dans le code HTML. De nombreux attributs existent pour différents éléments, mais ne fonctionnent pas avec toutes les balises. Cela peut créer des situations compliqués, car HTML ne génère pas de message d'erreur. Il est donc possible de se retrouver avec des attributs sur des éléments n'ayant aucun impact, amenant de la confusion et de la maintenance supplémentaire. Pour s'assurer de la justesse d'un code, il faut donc le valider à l'aide de linters, capable de remonter les erreurs au développeur.

Enfin, HTML est par nature statique : il est impossible de le modifier à l'exécution sans passer par JavaScript. Cela pose des problèmes compliqués pour gérer de multiples pages HTML : il faut forcément recharger la page avec un nouveau code HTML pour obtenir un affichage différent — gérer le DOM en JavaScript étant très compliqué à effectuer à cause des nombreux effets de bord induits.

## CSS

CSS, ou Cascading Style Sheet a été désigné au milieu des années 90, alors que le besoin de styliser la page web de l'utilisateur se faisait de plus en plus présent. Différentes technologies ont donc été essayé, avant de s'arrêter enfin sur CSS. Alors que HTML répond au besoin de structurer l'information présenté à l'utilisateur, CSS répond au besoin de mise en page de cette même information.

CSS a été pensé de manière a pouvoir évoluer de manière incrémentale. Chaque spécification de CSS est désigné comme un sur-ensemble de la spécification précédente. Cela permet à la fois une rétro-compatibilité parfaite et une évolution du standard au fil du temps. Toutefois, cela induit le fait que des morceaux de code auparavant utiles deviennent obosolètes avec l'évolution du standard. L'exemple récent des flexbox CSS ainsi que des grid remettent grandement en question la nécessité de recourir à des table ou des float. Cela permet certes un meilleur agencement de la page, mais cela force un développeur à se souvenir de l'ensemble des versions et des différentes possibilités pour réaliser une seule action, malgré l'évolution du standard.

De plus, là où CSS devrait prendre en charge le style de la page, le langage se développe de plus en plus pour prendre en charge la structure de la page&lt;, ou son comportement. Les attributs `position` ou `display` en sont le parfait exemple : ceux-ci influencent beaucoup la structure de la page : le changement d'un de ces attributs peut provoquer une réécriture complète de la page. À l'inverse, changer la police de caractères utilisée sur la page n'est qu'une modification mineure de la page, et se répercute très peu sur la structure de la page. On observe donc aujourd'hui un recoupement important entre CSS et HTML sur le concept de structure de la page. Il devient impossible de structurer sa page sans avoir recours à CSS.

On voit donc émerger les concepts de Functional CSS \([Tachyons CSS](https://github.com/tachyons-css/tachyons) en est le meilleur représentant\) ou d'[Atomic CSS](https://acss.io/), ceux-ci se concentrant principalement sur la réduction de la complexité de CSS. La méthodologie BEM cherche également à résoudre le même problème. Ceux-ci partent du principe qu'une classe CSS ne devrait contenir qu'une seule information \(on trouve ici le concept d'atomicité dans le sens où la classe, explicite, ne peut avoir de chevauchement de style dû à CSS et provoquer une erreur involontaire\). On multiplie alors les classes liées à un élément HTML pour appliquer plusieurs styles. Cela évite la confusion et la nécessité de rechercher, pour chaque classe, le style appliqué. 

```html
<button class="die-potato">
  "Not today…"
</button>

<button class="round shadow">
  "Noooooooo!"
</button>
```

```css
.die-potato {
  border-radius: 300px;
  box-shadow: 60px -16px teal;
}

.round {
  border-radius: 300px;
}

.shadow {
  box-shadow: 60px -16px teal;
}
```

Par exemple, ces deux boutons, bien qu'ils possèdent le même style, ne sont pas définis de la même manière. La multiplication des classes dans le deuxième cas permet de facilement réutiliser les différentes classes.

Pour finir, CSS pose d'autres problèmes, notamment au niveau de son exécution : il est très difficile d'identifier des problèmes de CSS, puisque son exécution est totalement silencieuse. Si une propriété est appliquée ou non, le navigateur ne renvoie aucune erreur. Cela ne facilite donc pas la recherche d'une erreur dans le code CSS lors de l'écrasement d'une propriété, ou de l'évolution du code. CSS ne prend pas non plus en charge les namespaces, ce qui force les utilisateurs à prêter attention à ne pas utiliser plusieurs fois un même nom, et à ne pas utiliser un nom déjà défini. Quant à l'utilisation des variables, cela est arrivé avec CSS3, mais cela requière une utilisation de JavaScript, ce qui augmente encore la complexité du langage.

Des initiatives comme [SASS](http://sass-lang.com/) \(pour Super Awesome StyleSheet\) tentent de corriger ces problèmes en introduisant namespaces ou variables, au prix d'une première phase de compilation des fichiers SASS en CSS.

## JavaScript

Le dernier des langages web, JavaScript a été inventé en 1995 par Brendan Eich pour le navigateur NetScape. Ce langage de script au typage faible est fortement orienté pour interagir parfaitement avec le DOM et les différentes technologies du web, notamment HTML et CSS.

JavaScript est un langage bâti sur le standard ECMA, l'ECMAScript. Le langage s'est vu standardisé tardivement, lorsque chaque navigateur implémentait sa propre variante de JavaScript pour pouvoir interagir avec les pages web, et permettre des animations sur ces mêmes pages. Cela a pendant longtemps entrainé le fait d'un comportement non fiable à travers les différents navigateurs, et force les développeurs à écrire le même code sous différentes formes pour respecter chacun des navigateurs et qu'un même code s'exécute de manière identique sur chaque navigateur ; ou à utiliser des bibliothèques prenant cela en charge pour eux, comme [jQuery](https://jquery.com/).

JavaScript possède aujourd'hui un environnement de développement très riche et est mis à jour régulièrement, mais possède encore aujourd'hui quelques faiblesses, notamment dans le support des nouvelles versions au sein des navigateurs, ce qui ne résout pas encore le problème historique de portée lexicale de JavaScript : JavaScript n'implémentant pas le concept de portée lexicale comme le fait aujourd'hui la quasi-totalité des langages de programmation. De plus, son absence de typage et de messages d'erreur en font un langage compliqué à maintenir, ce qui pousse de nombreuses personnes à utiliser des langages compilant vers JavaScript. Il est possible de citer dans cette catégorie PureScript, Elm, ClojureScript ou encore Dart et TypeScript. Mais de nombreuses initiatives tentent également d'améliorer JavaScript. On peut penser pour cela à Flow, le typeur de JavaScript par Facebook.

On peut observer une forte volonté aujourd'hui de se séparer de JavaScript dans le navigateur, notamment avec l'arrivée des langages de programmation fonctionnels compilant vers JavaScript. Toutefois, un changement de paradigme a été réellement nécessaire pour pouvoir utiliser des langages fonctionnels en frontend, les technologies actuelles étant principalement focalisées sur la maintenance du DOM par l'utilisateur, et son interaction avec JavaScript. Facebook fut le premier à populariser le concept du VirtualDOM, permettant de s'affranchir en grande partie des contraintes du web. Il est donc possible aujourd'hui de voir JavaScript comme étant l'assembleur du web, en attendant l'arrivée de WebAssembly dans l'ensemble des navigateurs.

