# React & Redux, le frontend fonctionnel

HTML et JavaScript ont toujours été extrêmement liés. Bien qu'artificiellement séparés l'un de l'autre, les deux sont indissociables l'un de l'autre : une modification du HTML amène à modifier le JavaScript, et une modification du JavaScript amène à modifier le HTML. Cela est dû au fait que JavaScript est intrinséquement couplé à HTML pour les modifications du DOM inhérentes au dynamisme souhaité par les sites web modernes. Le DOM étant représenté par HTML et manipulé en JavaScript, une modification dans l'un affecte l'autre. Cela a amené à réfléchir à une nouvelle organisation des langages, et une séparation des préocuppations \(_separation of concerns_\) différente. Plutôt qu'écrire du HTML puis le modifier en JavaScript, pourquoi ne pas écrire intégralement le DOM en JavaScript \(avec une représentation en mémoire en HTML évidemment\), et gérer ainsi toute la logique de l'interface directement en JavaScript ? Cela diminue ainsi le nombre de fichiers \(et de langages\) à modifier lors d'un changement de structure du document, et permet dans un premier temps de se passer de HTML pour l'écriture d'une application web.

## React, la stratégie du Virtual DOM

React est une réponse à cette problématique : se passer de HTML pour l'écriture du DOM, en déléguant cela à JavaScript. JavaScript dispose en effet de toutes les possibilités pour éditer, supprimer et créer des éléments dans le DOM. Toutefois, gérer manuellement le DOM est compliqué, et rapidement source d'erreurs.

L'idée principale de React est donc d'écrire des objets JavaScript produisant du HTML en fonction de ses données et de son état interne, et de mettre à jour automatiquement le HTML dans le DOM sans avoir à le modifier soi-même. Pour cela, React utilise une technologie de Virtual DOM. Le Virtual DOM consiste en un DOM virtuel stocké en RAM en JavaScript, en plus du DOM rendu par le navigateur. Lors de l'initialisation du programme, React exécute une première fois chacune des fonctions de vue du programme, et génère un premier DOM qu'il stocke en RAM, puis affiche. Le Virtual DOM se chargeant de faire la jonction entre la structure en mémoire et ce qui est affiché. Ensuite, à chaque évènement d'utilisateur, ou à chaque changement des données de l'application, les fonctions de création d'affichage recréent un DOM, qui est comparé avec le précédent DOM en mémoire. Un diff est alors effectué entre les DOM, et les modifications correspondantes sont répercutés sur le DOM rendu par le navigateur de manière automatique. Seul le strict minimum est rendu à chaque changement.

Il est ainsi possible d'isoler ces objets, ces composants — des web components — et de les faire fonctionner seuls, tant qu'ils sont capables de produire du HTML, et d'interagir avec les évènements JavaScript définis par le Virtual DOM.

Ainsi, React fonctionne sur un modèle comportant de nombreux effets de bords. En effet, un exemple simple de compteur en React peut se définir comme tel :

```jsx
class App extends React.Component {
  constructor(props) {
    this.state = {
      count: 0
    }
  }
  onClick(event) {
    this.setState({
      count: this.state.count + 1
    });
  }

  render() {
    return (
      <div>
        <h1>{this.state.count}</h1>
        <button onClick={this.onClick.bind(this)}>Augmentation</button>
      </div>
    )
  }
}

React.render(<App/>, document.getElementById('app-container'))
```

Cet exemple simple illustre la création d'un web-component \(App\), contenant un état comportant une variable entière _count_, un évènement sur le clic de l'utilisateur incrémentant le compteur, et enfin une fonction de rendu permettant de produire le HTML correspondant à chaque mise à jour de l'état du composant. Ce rendu correspond au HTML envoyé au Virtual DOM, et la syntaxe des composants React est effectuée en JSX. Comme il est possible de le voir, un tel composant repose fortement sur un effet de bord : l'incrémentation du compteur du composant.

Dans un modèle fonctionnel pur, un tel composant est inutilisable : l'intégralité des valeurs manipulées étant immuables, il est impossible d'effectuer de tels modifications. De plus, une multiplication de composants muables font perdre l'intérêt d'un langage de programmation fonctionnel immuable. Il est donc nécessaire de voir les fonctions de rendu comme des fonctions pures : celles-ci produisent du HTML en fonction de leur entrée, ces entrées étant indépendantes des composants dans lesquels résident les fonctions de rendu. Cela permet alors de conceptualiser un modèle sans effet de bord, et donc d'utiliser des langages fonctionnels, ce qui était jusque là compliqué.

## Redux, l'ajout du fonctionnel à JavaScript

Ce modèle sans effet de bord est implémenté à travers une extension de React : Redux. Ce modèle fonctionnel repose sur un état global de l'application se mettant à jour au gré des évènements arrivant dans l'application. Le même exemple de compteur donné en React, mais cette fois en Redux se conceptualise grossièrement ainsi :

```js
const counter = (state = 0, action) => {
  switch (action.type) {
    case 'INCREMENT':
      return state + 1
    case 'DECREMENT':
      return state - 1
    default:
      return state
  }
}

const store = createStore(counter)
const rootEl = document.getElementById('root')

const render = () => ReactDOM.render(
  <Counter
    value={store.getState()}
    onIncrement={() => store.dispatch({ type: 'INCREMENT' })}
    onDecrement={() => store.dispatch({ type: 'DECREMENT' })}
  />,
  rootEl
)
```

On observe ainsi de nombreux changements dans l'architecture du programme \(la fonction de rendu est volontairement absente ici, n'apportant aucune information pertinente\). Le premier changement est l'apparition d'un « store », celui-ci étant la seule partie variable de l'application. L'intégralité des variables et des modifications dans le programme vont directement dans le store. Ce store est mis à jour par le biais d'actions, dispatchés selon le message indiqué par l'action. On peut voir ici que le compteur envoie un message `INCREMENT` lorsque l'utilisateur souhaite l'augmenter et `DECREMENT` dans le cas contraire. Lors de l'émission de ces messages, ils passent dans la fonction counter, qui va alors distinguer les cas, et effectuer les modifications dans le store en fonction du message. Une fois le store mis à jour, un rendu est alors effectué et envoyé au Virtual DOM, pour effectuer un nouvel affichage dans le navigateur.

On peut donc voir ici que le seul moyen de modifier l'état de l'application est d'envoyer un message, qui sera réceptionné par Redux, qui pourra alors déclencher une mise à jour du store. On observe donc ici un pattern purement fonctionnel. Si le seul effet de bord généré par l'application est prise en charge par le langage, on peut donc se passer complètement des effets de bord et utiliser un langage fonctionnel en frontend.

