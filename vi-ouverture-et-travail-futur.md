# Conclusion et Travail Futur

## Conclusion

L'objectif consistant à réaliser une application web en se passant intégralement de HTML, CSS et JavaScript est atteint. Les nouveaux langages fonctionnels pouvant compiler vers JavaScript ouvrent de nombreuses possibilités pour le développement web, et l'arrivée prochaine de WebAssembly ne devrait faire qu'encore accentuer le tournant pris par le développement web en frontend. La technologie de Virtual DOM prend de plus en plus d'ampleur et permet aujourd'hui de se passer des langages impératifs pour écrire un site web.

## Travail Futur

Les futurs travaux en rapport continue sur la même lancée.

### Animation

La majorité des animations sur le web sont aujourd'hui faites à partir de JavaScript et d'animations CSS. Il est donc important d'écrire un framework capable de profiter au maximum des capacités des navigateurs modernes, mais aussi de définir ses propres animations. Il est donc nécessaire de voir ce qu'il est possible de faire à l'aide des animations et transitions CSS, ces dernières étant accéléré graphiquement par le GPU, mais aussi de regarder et essayer de voir ce qu'il est possible de faire en utilisant exclusivement Elm.

### Performances

Les performances du framework n'ont pas été testés, et il est nécessaire de faire des tests approfondis, sur Elm, puis peut-être à l'aide d'autres langages de programmation \(ClojureScript, PureScript\) afin de confirmer ou d'infirmer les résultats. Cela permettra de statuer sur la viabilité de la solution à long terme.

WebAssembly peut également être une piste intéressante, puisque les premières béta commencent à être implémentés dans les navigateurs, Firefox en tête.

### Meilleure distinction Structure/Style/Comportement

Améliorer la nouvelle séparation des tâches. Ce travail est aujourd'hui un sujet important dans le web pour pouvoir répondre aux problématiques posées par les langages de programmation actuels. 

Il est donc important de bien séparer les éléments de style des éléments de structure, et de fournir ces structures et layout dans un framework correspondant. Il est donc nécessaire d'implémenter une grille, des conteneurs, etc. Il est également nécessaire d'améliorer la structure de Style de la bibliothèque Elegant pour s'assurer d'excellentes performances et d'une consommation mémoire réduite.





