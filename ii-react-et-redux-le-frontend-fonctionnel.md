# React & Redux, le frontend fonctionnel

HTML et JavaScript ont toujours été extrêmement liés. Bien qu'artificiellement séparés l'un de l'autre, les deux sont indissociables l'un de l'autre : une modification du HTML amène à modifier le JavaScript, et une modification du JavaScript amène à modifier le HTML la majeure partie du temps. Cela est dû au fait que JavaScript est intrinséquement couplé à HTML pour les modifications du DOM inhérentes au dynamisme souhaité par les sites web modernes. Le DOM étant représenté par HTML et manipulé en JavaScript, une modification dans l'un affecte l'autre. Cela a amené à réfléchir à une nouvelle organisation des langages, et une séparation des préocuppations \(_separation of concerns_\) différente. Plutôt qu'écrire du HTML puis le modifier en JavaScript, pourquoi ne pas écrire intégralement le DOM en JavaScript \(avec une représentation en mémoire en HTML évidemment\), et gérer ainsi toute la logique de l'interface directement en JavaScript ? Cela diminue ainsi le nombre de fichiers \(et de langages\) à modifier lors d'un changement de structure du document.

## React, la stratégie du Virtual DOM

React est une réponse à cette problématique : se passer HTML pour l'écriture du DOM, en déléguant cela à JavaScript. JavaScript dispose en effet de toutes les possibilités pour éditer, supprimer et créer des éléments dans le DOM.

* Virtual DOM
* Side Effect
* Une vision sans Side Effect
  * Des fonctions pures

## Redux, l'ajout du fonctionnel à JavaScript

* Modèle fonctionnel
* Pas de side effect
* Un état évolue au fil du temps
* One way data flux
* Inspiré de Elm : pourquoi ne pas directement faire du Elm ?



