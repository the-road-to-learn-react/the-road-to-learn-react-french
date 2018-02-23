# Gestion de l'état au sein de React et au-delà

Vous avez déjà appris les basiques de la gestion d'état dans React dans les chapitres précédents. Ce chapitre creuse ce sujet un peu plus profondément. Vous apprendrez les meilleurs pratiques, comment les appliquer et pourquoi vous pouvez envisager l'usage d'une bibliothèque tierce de gestion d'état.

## L'élévation de l'état

Seule le composant App est un composant ES6 stateful dans votre application. Il gère beaucoup d'aspects de l'état et de la logique de l'application au sein de ses méthodes. Peut-être avez-vous remarqué que vous passez beaucoup de propriétés à votre composant Table. La plupart de ces propriétés sont seulement utilisées dans le composant Table. En conclusion une personne pourrez soutenir que c'est un non-sens que le composant App connaisse toutes ces propriétés.

L'entièreté de la fonctionnalité de tri est seulement utilisé dans le composant Table. Vous pourriez la migrer dans le composant Table, car le composant App n'en a pas du tout besoin. Le processus de refactoring en sous-état d'un composant vers un autre est connu comme *lifting state*. Dans votre cas, vous voulez migrer l'état qui n'est pas utilisé dans le composant App à l'intérieur du composant Table. L'état est déplacé vers le bas depuis le parent vers le composant enfant.

Dans le but de traiter avec l'état et les méthodes de classe dans le composant Table, il doit devenir un composant de classe ES6. Le refactoring d'un composant fonctionnel stateless vers un composant de classe ES6 est trivial.

Voici votre composant Table comme un composant fonctionnel stateless : 

{title="src/App.js",lang=javascript}

```js
const Table = ({
  list,
  sortKey,
  isSortReverse,
  onSort,
  onDismiss
}) => {
  const sortedList = SORTS[sortKey](list);
  const reverseSortedList = isSortReverse
    ? sortedList.reverse()
    : sortedList;

  return(
    ...
  );
}
```

Voici votre composant Table comme un composant de classe ES6 :

{title="src/App.js",lang=javascript}

```js
# leanpub-start-insert
class Table extends Component {
  render() {
    const {
      list,
      sortKey,
      isSortReverse,
      onSort,
      onDismiss
    } = this.props;

    const sortedList = SORTS[sortKey](list);
    const reverseSortedList = isSortReverse
      ? sortedList.reverse()
      : sortedList;

    return(
      ...
    );
  }
}
# leanpub-end-insert
```

Puisque vous voulez traiter l'état et les méthodes dans votre composant, vous devez ajouter un constructeur et un état initial.

{title="src/App.js",lang=javascript}

```js
class Table extends Component {
# leanpub-start-insert
  constructor(props) {
    super(props);

    this.state = {};
  }
# leanpub-end-insert

  render() {
    ...
  }
}
```

Maintenant vous pouvez migrer l'état et les méthodes de classe concernant la fonctionnalité de tri depuis votre composant App vers votre composant Table de dessous.

{title="src/App.js",lang=javascript}

```js
class Table extends Component {
  constructor(props) {
    super(props);

# leanpub-start-insert
    this.state = {
      sortKey: 'NONE',
      isSortReverse: false,
    };

    this.onSort = this.onSort.bind(this);
# leanpub-end-insert
  }

# leanpub-start-insert
  onSort(sortKey) {
    const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
    this.setState({ sortKey, isSortReverse });
  }
# leanpub-end-insert

  render() {
    ...
  }
}
```

N'oubliez pas de supprimer l'état déplacé et la méthode de classe `onSort()` de votre composant App.

{title="src/App.js",lang=javascript}

```js
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      results: null,
      searchKey: '',
      searchTerm: DEFAULT_QUERY,
      error: null,
      isLoading: false,
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
  }

  ...

}
```

De plus, vous pouvez faire une API allégée du composant Table. Supprimez les propriétés qui lui étaient passées depuis le composant App, car elles sont maintenant gérées dans le composant Table de façon interne.

{title="src/App.js",lang=javascript}
```js
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading
    } = this.state;
# leanpub-end-insert

    ...

    return (
      <div className="page">
        ...
# leanpub-start-insert
        <Table
          list={list}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
        ...
      </div>
    );
  }
}
```

Maintenant dans votre composant Table vous pouvez utiliser la méthode interne `onSort()` et l'état interne de Table.

{title="src/App.js",lang=javascript}
```js
class Table extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      list,
      onDismiss
    } = this.props;

    const {
      sortKey,
      isSortReverse,
    } = this.state;
# leanpub-end-insert

    const sortedList = SORTS[sortKey](list);
    const reverseSortedList = isSortReverse
      ? sortedList.reverse()
      : sortedList;

    return(
      <div className="table">
        <div className="table-header">
          <span style={{ width: '40%' }}>
            <Sort
              sortKey={'TITLE'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Title
            </Sort>
          </span>
          <span style={{ width: '30%' }}>
            <Sort
              sortKey={'AUTHOR'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Author
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            <Sort
              sortKey={'COMMENTS'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Comments
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            <Sort
              sortKey={'POINTS'}
# leanpub-start-insert
              onSort={this.onSort}
# leanpub-end-insert
              activeSortKey={sortKey}
            >
              Points
            </Sort>
          </span>
          <span style={{ width: '10%' }}>
            Archive
          </span>
        </div>
        { reverseSortedList.map((item) =>
          ...
        )}
      </div>
    );
  }
}
```

Votre application doit encore fonctionner. Mais vous avez fait un refactoring crucial. Vous avez déplacé la fonctionnalité et l'état plus près dans un autre composant. Les autres composants ont été allégés. De plus l'API du composant Table a été allégé car il traite la fonctionnalité de tri de façon interne.

Le processus de *lifting state* peut aller dans l'autre sens également : depuis le composant enfant vers le composant parent. Cela s'appelle le *lifting state up*. Imaginez-vous vous occupiez également de l'état interne dans un composant enfant. Maintenant vous devriez satisfaire également un besoin de montrer l'état dans votre composant parent. Vous devriez élever l'état vers votre composant parent. Mais cela va plus loin. Imaginez vous souhaitez montrer l'état dans un composant frère de votre composant enfant. De nouveau vous devriez élever l'état vers votre composant parent. Le composant parent traite l'état interne, mais l'expose vers les deux composants enfants.

### Exercises:

* lire plus à propos du [lifting state dans React](https://facebook.github.io/react/docs/lifting-state-up.html)
* lire plus à propos du lifting state dans [React avant l'utilisation de Redux](https://www.robinwieruch.de/learn-react-before-using-redux/)

## setState() : revisité

Jusqu'ici vous devez utiliser `setState()` de React pour gérer votre état de composant interne. Vous pouvez transmettre un objet à une fonction qui vous permet de mettre à jour partiellement l'état interne.

{title="Code Playground",lang="javascript"}
```js
this.setState({ foo: bar });
```

Mais `setState()` ne prend pas seulement un objet. Dans sa seconde version, vous pouvez transmettre une fonction pour mettre à jour l'état.

{title="Code Playground",lang="javascript"}
```js
this.setState((prevState, props) => {
  ...
});
```

Pourquoi vouloir faire cela? Il y a un des cas d'utilisation cruciaux où il fait sens d'utiliser une fonction à la place d'un objet. C'est quand vous mettez à jour l'état en fonction de votre état précédent ou des propriétés. Si vous n'utilisez pas une fonction, la gestion interne d'état peut engendrer des bugs.

Mais pourquoi cela cause des bugs d'utiliser un objet à la place d'une fonction lorsque la mise à jour dépend de l'état précédent ou des propriétés? La méthode `setState()` de React est asynchrone. React regroupe les appelles de `setState()` et les exécute finalement. Il peut arriver que l'état précédent ou que les propriétés soient modifiés entre-temps alors vous souhaitez vous fier à eux dans votre appelle `setState()`.

{title="Code Playground",lang="javascript"}
```js
const { fooCount } = this.state;
const { barCount } = this.props;
this.setState({ count: fooCount + barCount });
```

Imaginez `fooCount` et `barCount`, c'est-à-dire l'état ou les propriétés, changent autre part de façon asynchrone lorsque vous appelez `setState()`. Dans une application grandissante, vous avez plus qu'un appelle à `setState()` au travers de votre application. Puisque `setState()` s'exécute de façon asynchrone, vous pourriez dépendre dans cet exemple de valeurs périmées.

Avec l'approche par fonction, la fonction dans `setState()` est une fonction de rappel (callback) qui opère sur l'état et les propriétés au moment de l'exécution de la fonction de rappel. Bien que `setState()` soit asynchrone, avec une fonction il prend l'état et les propriétés au moment de quand c'était exécuté.

{title="Code Playground",lang="javascript"}
```js
this.setState((prevState, props) => {
  const { fooCount } = prevState;
  const { barCount } = props;
  return { count: fooCount + barCount };
});
```

Maintenant, retournons à votre code pour réparer ce comportement. Ensemble nous allons le réparer à un endroit où `setState()` est utilisé et dépend de l'état et des propriétés. Après quoi, vous serez capable de l'appliquer à d'autres endroits également.

La méthode `setSearchTopStories()` dépend de l'état précédent c'est donc un parfait exemple pour utiliser une fonction à la place d'un objet dans `setState()`. Pour l'heure, cela ressemble au bout de code suivant.

{title="src/App.js",lang=javascript}
```js
setSearchTopStories(result) {
  const { hits, page } = result;
  const { searchKey, results } = this.state;

  const oldHits = results && results[searchKey]
    ? results[searchKey].hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  this.setState({
    results: {
      ...results,
      [searchKey]: { hits: updatedHits, page }
    },
    isLoading: false
  });
}
```

Vous extrayez les valeurs de l'état, mais vous mettez à jour l'état en fonction de l'état précédent de façon asynchrone.

{title="src/App.js",lang=javascript}
```js
setSearchTopStories(result) {
  const { hits, page } = result;

# leanpub-start-insert
  this.setState(prevState => {
    ...
  });
# leanpub-end-insert
}
```

Vous pouvez déplacer le bloc en entier que vous avez déjà implémenté à l'intérieur de la fonction. Vous avez seulement à échanger le fait que vous opérez sur le `prevState` plutôt que `this.state`.

{title="src/App.js",lang=javascript}
```js
setSearchTopStories(result) {
  const { hits, page } = result;

  this.setState(prevState => {
# leanpub-start-insert
    const { searchKey, results } = prevState;

    const oldHits = results && results[searchKey]
      ? results[searchKey].hits
      : [];

    const updatedHits = [
      ...oldHits,
      ...hits
    ];

    return {
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      },
      isLoading: false
    };
# leanpub-end-insert
  });
}
```

Cela réparera notre problème d'état périmé. Il y a une amélioration supplémentaire. Puisque c'est une fonction, vous pouvez extraire la fonction pour un gain de lisibilité. C'est l'un des avantages d'utiliser une fonction au lieu d'un objet. La fonction peut vivre à l'extérieur du composant. Mais vous devez utiliser une fonction d'ordre supérieur pour transmettre le résultat. En fin de compte, vous souhaitez mettre à jour l'état basé sur le résultat rapporté de l'API.

{title="src/App.js",lang=javascript}
```js
setSearchTopStories(result) {
  const { hits, page } = result;
  this.setState(updateSearchTopStoriesState(hits, page));
}
```

La fonction `updateSearchTopStoriesState()`retourne une fonction. C'est une fonction d'ordre supérieur. Vous pouvez définir cette fonction d'ordre supérieur à l'extérieur de votre composant App. Notez comment la signature de la fonction change légèrement.

{title="src/App.js",lang=javascript}
```js

# leanpub-start-insert
const updateSearchTopStoriesState = (hits, page) => (prevState) => {
  const { searchKey, results } = prevState;

  const oldHits = results && results[searchKey]
    ? results[searchKey].hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  return {
    results: {
      ...results,
      [searchKey]: { hits: updatedHits, page }
    },
    isLoading: false
  };
};
# leanpub-end-insert

class App extends Component {
  ...
}
```

C'est tout. La fonction à la place de l'approche objet dans `setState()` résout de potentiels bugs et améliore la lisibilité et maintenabilité de votre code. De plus, elle devient testable à l'extérieur du composant App. Vous pouvez l'exporter et écrire un test pour vous exercer.

### Exercise:

* lire plus à propos de l'[utilisation correcte de l'état dans React](https://facebook.github.io/react/docs/state-and-lifecycle.html#using-state-correctly)
* refactorer toutes les méthodes `setState()` utiliser une fonction
  * mais seulement quand cela fait sens, c'est-à-dire lorsqu'il dépend de l'état ou des propriétés
* lancer vos tests de nouveau et vérifier que tout est à jour

## Apprivoiser l'état

Les chapitres précédents vous ont montrés que la gestion de l'état peut être un sujet crucial dans les applications imposantes. De façon générale, pas seulement React mais beaucoup de frameworks SPA luttent avec çà. Les applications deviennent plus complexes depuis quelques années. Un des gros challenges dans les applications web de nos jours est d'apprivoiser et contrôler l'état.

Comparé aux autres solutions, React à déjà fait un grand pas en avant. Le flux de données unidirectionnel et une API simple pour gérer l'état dans un composant sont indispensables. Ces concepts le rendent plus simple pour raisonner au sujet de votre état ainsi que de vos changements d'états. Cela rend plus simple de raisonner au niveau composant et d'un certain point au niveau applicatif.

Dans une application grandissante, cela devient plus difficile de raisonner au sujet des changements d'état. Vous pouvez introduire des bugs en agissant sur des états périmés lors de l'utilisation d'un objet à la place d'une fonction dans `setState()`. Vous devez élever l'état pour partager le nécessaire ou cacher l'état non nécessaire au travers des composants. Il peut arriver qu'un composant est besoin d'élever son état vers le haut, car ses composants frères dépendent de ce dernier. Peut-être le composant est éloigné dans l'arbre de composant et ainsi vous devrez partager l'état au travers de l'ensemble de l'arbre de composant. En conclusion les composants seront impliqués dans la gestion d'état dans de bien plus grandes mesures. Mais après tout, la principale responsabilité des composants devraient être la représentation de l'UI (interface utilisateur), n'est ce pas?

Pour toutes ces raisons, il existe des solutions autonomes pour prendre en charge la gestion de l'état. Ces solutions ne sont pas seulement utilisées dans React. Cependant, cela fait de l'écosystème React un outil extrêmement puissant. Vous pouvez utiliser différentes solutions pour résoudre vos problèmes. Confier le problème de la mise à l'échelle de la gestion de l'état, vous pourriez avoir entendu parler des bibliohtèques [Redux](http://redux.js.org/docs/introduction/) ou [MobX](https://mobx.js.org/). Vous pouvez utiliser l'une de ces solutions dans une application React. Elles arrivent avec des extensions, [react-redux](https://github.com/reactjs/react-redux) et [mobx-react](https://github.com/mobxjs/mobx-react), pour les intégrer à l'intérieur de la couche vue de React.

Redux et MobX sont en dehors du cadre de ce livre. Lorsque vous avez fini le livre, vous obtiendrez des conseils sûrs comment vous pouvez continuer à apprendre React et son écosystème. Un cheminement d'apprentissage pourrait être l'apprentissage de Redux. Avant que vous plongez dans le sujet de la gestion d'état externe, je peux vous recommander de lire cette [article](https://www.robinwieruch.de/redux-mobx-confusion/). Il a pour but de vous donner une meilleure compréhension de comment apprendre la gestion d'état externe.

### Exercices :

* lire plus à propos de la [gestion d'état externe et de comment l'apprendre](https://www.robinwieruch.de/redux-mobx-confusion/)
* regarder mon second livre à propos de la [gestion de l'état dans React](https://roadtoreact.com/)

{pagebreak}

Vous avez appris la gestion d'état avancé dans React! Récapitulons les derniers chapitres :

* React
  * élévation de la gestion de l'état haut et bas vers les composants appropriés
  * setState peut utiliser une fonction pour empêcher des bugs d'état périmé
  * les solutions externes existantes qui vous aident à apprivoiser l'état

Vous pouvez trouver le code source sur le [dépôt officiel](https://github.com/rwieruch/hackernews-client/tree/4.6).
