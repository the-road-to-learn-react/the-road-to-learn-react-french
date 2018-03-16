# React composants avancés

Le chapitre se concentrera sur l'implémentation de composants React avancés. Vous apprendrez à propos des composants d'ordre supérieur et comment les implémenter. De plus, vous plongerez dans des sujets plus avancés de React et implémenterez des interactions complexes avec ce dernier.

## Ref un DOM Element

Parfois vous aurez besoin d'interagir avec votre noeud de DOM au sein de React. L'attribut `ref` vous donne un accès à un noeud de vos éléments. Habituellement c'est un anti-pattern en React, car vous devez l'utiliser sa manière déclarative pour faire quoi que ce soit ainsi que son flux de données unidirectionnel. Vous avez appris cela lorsque vous avez introduit votre premier champ d'entrée de recherche. Mais il y a certains cas où vous avez besoin d'accéder à un noeud de DOM. La documentation officielle mentionne trois cas : 

* utiliser l'API du DOM (focus, media playback etc.)
* invoquer des animations impératives de noeuds du DOM
* intégrer une librairie tierce qui a besoin de noeuds du DOM (ex : [D3.js](https://d3js.org/))

Réalisons un exemple avec le composant Search. Lorsque l'application se rend pour la première fois, le champ d'entrée devra être en focus. C'est l'un des cas d'utilisation où vous aurez besoin d'accéder à l'API du DOM. Ce chapitre vous montre comment cela fonctionne, mais puisque c'est ne pas très utile pour l'application en soi, nous omettrons les changements dans le prochain chapitre. Néanmoins vous pouvez les conserver pour votre propre application.

En général, vous pouvez utiliser l'attribut `ref` à la fois dans les composants fonctionnels stateless et les composants de classe ES6. Dans le cas de la fonctionnalité de focus, vous aurez besoin des méthodes du cycle de vie. C'est pourquoi l'approche est en premier lieu présentée en utilisant l'attribut `ref` avec un composant de classe ES6.

L'étape initiale consiste à refactoriser le composant fonctionnel stateless en un composant de classe ES6.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
class Search extends Component {
  render() {
    const {
      value,
      onChange,
      onSubmit,
      children
    } = this.props;

    return (
# leanpub-end-insert
      <form onSubmit={onSubmit}>
        <input
          type="text"
          value={value}
          onChange={onChange}
        />
        <button type="submit">
          {children}
        </button>
      </form>
# leanpub-start-insert
    );
  }
}
# leanpub-end-insert
~~~~~~~~

L'objet `this` du composant de classe ES6 nous aide à référencer le noeud du DOM avec l'attribut `ref`.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Search extends Component {
  render() {
    const {
      value,
      onChange,
      onSubmit,
      children
    } = this.props;

    return (
      <form onSubmit={onSubmit}>
        <input
          type="text"
          value={value}
          onChange={onChange}
# leanpub-start-insert
          ref={(node) => { this.input = node; }}
# leanpub-end-insert
        />
        <button type="submit">
          {children}
        </button>
      </form>
    );
  }
}
~~~~~~~~

Maintenant vous pouvez focus le champ d'entrée lorsque le composant est monté en utilisant l'objet `this`, la méthode du cycle de vie appropriée, et l'API du DOM.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Search extends Component {
# leanpub-start-insert
  componentDidMount() {
    if(this.input) {
      this.input.focus();
    }
  }
# leanpub-end-insert

  render() {
    const {
      value,
      onChange,
      onSubmit,
      children
    } = this.props;

    return (
      <form onSubmit={onSubmit}>
        <input
          type="text"
          value={value}
          onChange={onChange}
          ref={(node) => { this.input = node; }}
        />
        <button type="submit">
          {children}
        </button>
      </form>
    );
  }
}
~~~~~~~~

Le champ d'entrée doit être focus lorsque l'application se rend. C'est essentiellement tout pour l'usage de l'attribut `ref`.

Mais comment vous aurez accès au `ref` dans un composant fonctionnel stateless sans l'objet `this`? Le composant fonctionnel stateless suivant en fait une démonstration :

{title="src/App.js",lang=javascript}
~~~~~~~~
const Search = ({
  value,
  onChange,
  onSubmit,
  children
}) => {
# leanpub-start-insert
  let input;
# leanpub-end-insert
  return (
    <form onSubmit={onSubmit}>
      <input
        type="text"
        value={value}
        onChange={onChange}
# leanpub-start-insert
        ref={(node) => input = node}
# leanpub-end-insert
      />
      <button type="submit">
        {children}
      </button>
    </form>
  );
}
~~~~~~~~

Maintenant vous serez capable d'accéder à l'entrée de l'élément du DOM. Dans l'exemple de la fonctionnalité du focus cela ne vous aidera pas, car vous ne disposez pas des méthodes du cycle de vie dans un composant fonctionnel stateless pour déclencher le focus. Mais dans un futur vous pourrez traverser d'autres cas d'utilisation où il y aura du sens d'utiliser un composant fonctionnel stateless avec l'attribut `ref`.

### Exercises

* lire plus à propos de l'[usage de l'attribut ref au sein de React](https://www.robinwieruch.de/react-ref-attribute-dom-node/)
* lire plus à propos de l'[attribut ref de façon générale au sein de React](ttps://reactjs.org/docs/refs-and-the-dom.html)

## Chargement ...

Retournons à l'application. Vous pourriez vouloir afficher un indicateur de chargement lorsque vous soumettez une requête de recherche à l'API d'Hacker News. La requête est asynchrone et vous devrez montrer à votre utilisateur quelques retours visuels que quelque chose se produit. Définissons un composant Loading réutilisable dans votre fichier *src/App.js*.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Loading = () =>
  <div>Loading ...</div>
~~~~~~~~

Maintenant vous aurez besoin d'une propriété pour stocker l'état du chargement. Selon l'état du chargement vous pouvez décider de montrer le composant Loading par la suite.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  _isMounted = false;

  constructor(props) {
    super(props);

    this.state = {
      results: null,
      searchKey: '',
      searchTerm: DEFAULT_QUERY,
      error: null,
# leanpub-start-insert
      isLoading: false,
# leanpub-end-insert
    };

    ...
  }

  ...

}
~~~~~~~~

La valeur initiale de votre propriété `isLoading` est fausse. Vous ne chargez rien avant que le composant App soit monté.

Lorsque vous faites la requête, vous mutez l'état de chargement à true. Finalement la requête réussira et vous pourrez muter l'état de chargement à false.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  setSearchTopStories(result) {
    ...

    this.setState({
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      },
# leanpub-start-insert
      isLoading: false
# leanpub-end-insert
    });
  }

  fetchSearchTopStories(searchTerm, page = 0) {
# leanpub-start-insert
    this.setState({ isLoading: true });
# leanpub-end-insert

    axios(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(result => this._isMounted && this.setSearchTopStories(result.data))
      .catch(error => this._isMounted && this.setState({ error }));
  }

  ...

}
~~~~~~~~

Dernière étape, vous utiliserez le composant Loading dans votre App. Un rendu conditionnel basé sur l'état de chargement décidera si vous affichez le composant Loading ou le composant Button. Ce dernier est votre bouton pour aller chercher plus de données.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error,
# leanpub-start-insert
      isLoading
# leanpub-end-insert
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <div className="interactions">
# leanpub-start-insert
          { isLoading
            ? <Loading />
            : <Button
              onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
              More
            </Button>
          }
# leanpub-end-insert
        </div>
      </div>
    );
  }
}
~~~~~~~~

Initialement le composant Loading s'affichera lorsque vous démarrer votre application, car vous ferez une requête avec `componentDidMount()`. Il n'y a pas de composant Table, car le liste est vide. Lorsque la réponse revient de l'API d'Hacker News, le résultat est montré, le chargement d'état est muté à false et le composant Loading disparaît. À la place, le bouton "More" pour aller chercher plus de données apparaît. Une fois que vous recherchez plus de données, le bouton disparaîtra de nouveau et le composant Loading se révèlera.

### Exercices :

* utiliser une biblohtèque telle que [Font Awesome](http://fontawesome.io/) pour afficher une icône de chargement au lieu du texte "Loading ..."

## Composants d'ordre supérieur

Les composants d'ordre supérieur (HOC) sont un concept avancé en React. Les HOCs sont un équivalent des fonctions d'ordre supérieur. Ils prennent plusieurs entrées - la plupart du temps un composant, mais aussi des arguments optionnels - et retournent un composant en sortie. Le composant retourné est une version améliorée du composant d'entrée et peut-être utilisé dans votre JSX.

Les HOCs sont utilisés pour différents cas d'utilisation. Ils préparent les propriétés, gère l'état où altère la représentation d'un composant. Un cas d'utilisation pourrait être d'utiliser un HOC en tant qu'assistant pour un rendu conditionnel. Imaginez-vous avez un composant List qui rend une liste d'éléments ou rien, car la liste est vide ou null. L'HOC pourrait protéger le fait que la liste souhaiterait ne rien rendre lorsqu'il n'y a pas de liste. À l'inverse, le sobre composant List n'a plus besoin de s'encombrer d'une liste non existante. Il se préoccupe seulement de la liste rendue.

Faisons un simple HOC qui prend un composant en entrée et retourne un composant. Vous pouvez le placer à l'intérieur de votre fichier *src/App.js*.

{title="src/App.js",lang=javascript}
~~~~~~~~
function withFoo(Component) {
  return function(props) {
    return <Component { ...props } />;
  }
}
~~~~~~~~

Une convention cohérente est de préfixer le nommage d'HOC avec `with`. Puisque vous utilisez du JavaScript ES6, vous pouvez exprimer l'HOC plus concisément avec une fonction fléchée ES6.

{title="src/App.js",lang=javascript}
~~~~~~~~
const withFoo = (Component) => (props) =>
  <Component { ...props } />
~~~~~~~~

Dans l'exemple, le composant d'entrée devrait rester le même que le composant de sortie. Rien ne se produit. Il rend la même instance de composant et passe toutes les propriétés au composant de sortie. Mais c'est inutile. Améliorons le composant de sortie. Le composant de sortie devrait afficher le composant Loading, quand l'état de chargement est à true, autrement il devrait afficher le composant d'entrée. Un rendu conditionnel est un bon cas d'utilisation pour un HOC.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const withLoading = (Component) => (props) =>
  props.isLoading
    ? <Loading />
    : <Component { ...props } />
# leanpub-end-insert
~~~~~~~~

Basé sur la propriété de chargement vous pouvez appliquer un rendu conditionnel. La fonction retournera le composant de chargement ou le composant d'entrée.

En général il peut être efficace de diffuser un objet, tel que les propriétés d'objet dans l'exemple précédent, en tant qu'entrée pour un composant. Observez la différence avec le bout de code suivant.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// avant vous aurez déstructuré les propriétés avant de les transmettre
const { foo, bar } = props;
<SomeComponent foo={foo} bar={bar} />

// Mais vous pouvez utiliser le spread operator objet pour transmettre toutes les propriétés de l'objet.
<SomeComponent { ...props } />
~~~~~~~~

Il y a une petite chose que vous devez éviter. Vous passez toutes les propriétés incluant la propriété `isLoading`, en diffusant l'objet, à l'intérieur du composant d'entrée. Cependant, le composant d'entrée pourrait ne pas désirer la propriété `isLoading`. Vous pouvez utiliser la décomposition du reste en ES6 pour éviter cela.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading
    ? <Loading />
    : <Component { ...rest } />
# leanpub-end-insert
~~~~~~~~

Cela prend une propriété à l'extérieur de l'objet, mais conserve le reste de l'objet. Cela fonctionne avec plusieurs propriétés également. Vous pourriez l'avoir déjà lu dans l'[affectation par décomposition](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment).

Maintenant vous pouvez utiliser l'HOC dans votre JSX. Un cas d'utilisation au sein de l'application pourrait être d'afficher soit le bouton "More" soit le composant Loading. Le composant Loading est toujours encapsulé dans l'HOC, mais un composant en entrée est manquant. Dans le cas d'utilisation consistant à montrer un composant Button ou un composant Loading, le Button est le composant d'entrée de l'HOC. Le composant de sortie améliorée est un composant ButtonWithLoading.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Button = ({
  onClick,
  className = '',
  children,
}) =>
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

const Loading = () =>
  <div>Loading ...</div>

const withLoading = (Component) => ({ isLoading, ...rest }) =>
  isLoading
    ? <Loading />
    : <Component { ...rest } />

# leanpub-start-insert
const ButtonWithLoading = withLoading(Button);
# leanpub-end-insert
~~~~~~~~

Maintenant tout est défini. En dernier étape, vous devez utiliser le composant ButtonWithLoading, qui reçoit l'état de chargement en tant que propriété additionnelle. Tandis que l'HOC consomme la propriété loading, tout autres propriétés seront transmises au composant Button.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    ...
    return (
      <div className="page">
        ...
        <div className="interactions">
# leanpub-start-insert
          <ButtonWithLoading
            isLoading={isLoading}
            onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
            More
          </ButtonWithLoading>
# leanpub-end-insert
        </div>
      </div>
    );
  }
}
~~~~~~~~

Lorsque vous lancez de nouveau vos tests, vous remarquerez que votre test d'instantané pour le composant App échoue. La différence devrait ressembler à cela sur le terminal :

{title="Command Line",lang="text"}
~~~~~~~~
-    <button
-      className=""
-      onClick={[Function]}
-      type="button"
-    >
-      More
-    </button>
+    <div>
+      Loading ...
+    </div>
~~~~~~~~

Vous pouvez maintenant soit réparer le composant, lorsque vous pensez qu'il y a quelques soucis avec cela, ou vous pouvez accepter le nouvel instantané. Parceque vous avez introduit le composant Loading dans ce chapitre, vous pouvez accepter la version modifiée du test d'instantané sur le terminal dans le test interactif.

Les composants d'ordre supérieur sont une technique avancée dans React. Ils ont de multiples buts comme améliorer la réutilisabilité des composants, une meilleure abstraction, une composabilité des composants et la manipulation des propriétés ainsi que de l'état et la vue. Ne vous inquiétez pas si vous ne les comprenez pas immédiatement. Cela prend du temps à s'habituer à eux.

Je vous encourage à lire la [véritable introduction aux composants d'ordre supérieur](https://www.robinwieruch.de/gentle-introduction-higher-order-components/). Il vous fournit une autre approche pour les apprendre, vous montre une façon élégante de les utiliser dans une approche de programmation fonctionnelle et résout spécifiquement le problème de rendu conditionnel avec les composants d'ordre supérieur.

### Exercices :

* lire la [véritable introduction aux composants d'ordre supérieur](https://www.robinwieruch.de/gentle-introduction-higher-order-components/)
* experimenter avec l'HOC que vous avez créé
* penser à un cas d'utilisation où un autre HOC ferait sens
  * implementer l'HOC, s'l y a un cas d'utilisation

## Tri avancé

Vous avez déjà implémenté une interaction de recherche côté client et serveur. Puisque vous avez un composant Table, il ferait sens d'améliorer le Table avec des interactions avancées. Pourquoi ne pas introduire une fonctionnalité de tri pour chaque colonne en utilisant les en-têtes de colonnes du Table?

Il serait possible d'écrire votre propre fonction `sort`, mais personnellement je préfère utiliser une bibliothèque pour ce genre de cas. [Lodash](https://lodash.com/) est l'une de ces bibliothèques utiles, mais vous pouvez utiliser n'importe quelle bibliothèque qui vous correspond. Installons Lodash et utilisons la pour la fonctionnalité de tri.

{title="Command Line",lang="text"}
~~~~~~~~
npm install lodash
~~~~~~~~

Maintenant vous pouvez importer la fonctionnalité de tri de Lodash dans votre fichier *src/App.js*.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import axios from 'axios';
# leanpub-start-insert
import { sortBy } from 'lodash';
# leanpub-end-insert
import './App.css';
~~~~~~~~

Vous avez plusieurs colonnes dans votre Table. Il y a les colonnes title, author, comments et points. Vous pouvez définir les fonctions de tri ainsi chaque fonction prend une liste et retourne une liste d'éléments triée en spécifiant la propriété. Qui plus est, vous aurez besoin d'une fonction de tri par défaut qui ne triera pas mais seulement retournera la liste non triée. Cela sera votre état initial.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

# leanpub-start-insert
const SORTS = {
  NONE: list => list,
  TITLE: list => sortBy(list, 'title'),
  AUTHOR: list => sortBy(list, 'author'),
  COMMENTS: list => sortBy(list, 'num_comments').reverse(),
  POINTS: list => sortBy(list, 'points').reverse(),
};
# leanpub-end-insert

class App extends Component {
  ...
}
...
~~~~~~~~

Vous pouvez voir qu'il y a deux fonctions de tri qui retournent une liste inversée. C'est parce que vous voulez voir les éléments avec le plus de commentaires et de points plutôt que de voir les éléments avec le total le plus faible lorsque vous triez la liste pour la première fois.

Maintenant l'objet `SORTS` vous permet de référencer toutes fonctions de tri.

Encore une fois votre composant App est responsable de stocker l'état du tri. L'état initial sera la fonction de tri initial par défaut, qui trie pas du tout mais retourne la liste d'entrée en sortie.

{title="src/App.js",lang=javascript}
~~~~~~~~
this.state = {
  results: null,
  searchKey: '',
  searchTerm: DEFAULT_QUERY,
  error: null,
  isLoading: false,
# leanpub-start-insert
  sortKey: 'NONE',
# leanpub-end-insert
};
~~~~~~~~

Une fois que vous choisissez un `sortKey` différent, admettons la clé `AUTHOR`, vous trierez la liste avec la fonction de tri approprié issu de l'objet `SORTS`.

Maintenant vous pouvez définir une nouvelle méthode de classe dans votre composant App qui simplement mute un `sortKey` vers votre état local du composant. Après quoi, le `sortKey` peut être utilisé pour retrouver la fonction de tri pour l'appliquer sur votre liste.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  _isMounted = false;

  constructor(props) {

    ...

    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
# leanpub-start-insert
    this.onSort = this.onSort.bind(this);
# leanpub-end-insert
  }

  ...

# leanpub-start-insert
  onSort(sortKey) {
    this.setState({ sortKey });
  }
# leanpub-end-insert

  ...

}
~~~~~~~~

La prochaine étape est de passer la méthode et `sortKey` à votre composant Table.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading,
# leanpub-start-insert
      sortKey
# leanpub-end-insert
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <Table
          list={list}
# leanpub-start-insert
          sortKey={sortKey}
          onSort={this.onSort}
# leanpub-end-insert
          onDismiss={this.onDismiss}
        />
        ...
      </div>
    );
  }
}
~~~~~~~~

Le composant Table est responsable de trier votre liste. Il prend une des fonctions de `SORT` à l'aide de `sortkey` et passe la liste d'entrée. Après quoi il conserve le mappage sur la liste triée.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Table = ({
  list,
  sortKey,
  onSort,
  onDismiss
}) =>
# leanpub-end-insert
  <div className="table">
# leanpub-start-insert
    {SORTS[sortKey](list).map(item =>
# leanpub-end-insert
      <div key={item.objectID} className="table-row">
        ...
      </div>
    )}
  </div>
~~~~~~~~

En théorie la liste devrait être triée par l'une des fonctions. Mais le tri par défaut est réglé à `NONE`, donc rien n'est encore trié. Jusqu'ici, personne éxecute la méthode `onSort()` pour changer le `sortKey`. Étendons la Table avec une ligne d'en-tête de colonne qui utilise les composants Sort dans les colonnes pour trier chaque colonne.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({
  list,
  sortKey,
  onSort,
  onDismiss
}) =>
  <div className="table">
# leanpub-start-insert
    <div className="table-header">
      <span style={{ width: '40%' }}>
        <Sort
          sortKey={'TITLE'}
          onSort={onSort}
        >
          Title
        </Sort>
      </span>
      <span style={{ width: '30%' }}>
        <Sort
          sortKey={'AUTHOR'}
          onSort={onSort}
        >
          Author
        </Sort>
      </span>
      <span style={{ width: '10%' }}>
        <Sort
          sortKey={'COMMENTS'}
          onSort={onSort}
        >
          Comments
        </Sort>
      </span>
      <span style={{ width: '10%' }}>
        <Sort
          sortKey={'POINTS'}
          onSort={onSort}
        >
          Points
        </Sort>
      </span>
      <span style={{ width: '10%' }}>
        Archive
      </span>
    </div>
# leanpub-end-insert
    {SORTS[sortKey](list).map(item =>
      ...
    )}
  </div>
~~~~~~~~

Chaque composant Sort dispose d'une `sortKey` spécifique et de la fonction générale `onSort()`. Internement cela appelle la méthode avec la `sortKey` pour établir la clé spécifique.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Sort = ({ sortKey, onSort, children }) =>
  <Button onClick={() => onSort(sortKey)}>
    {children}
  </Button>
~~~~~~~~

Comme vous pouvez le voir, le composant Sort réutilise votre composant Button commun. Au clic d'un bouton chaque individuelle `sortKey` passée sera mutée par la méthode `onSort()`. Maintenant vous devez être capable de trier la liste lorsque vous cliquez sur les en-têtes de colonne.

Il y a une amélioration mineure pour un visuel amélioré. Jusqu'ici, le bouton dans la colonne d'en-tête est un peu en décalé. Donnons au bouton dans le composant Sort un `className` approprié.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Sort = ({ sortKey, onSort, children }) =>
# leanpub-start-insert
  <Button
    onClick={() => onSort(sortKey)}
    className="button-inline"
  >
# leanpub-end-insert
    {children}
  </Button>
~~~~~~~~

Maintenant, il doit avoir un ravissant aspect. Le prochain but serait d'également implémenter un tri inverse. La liste doit inverser le tri dès que vous cliquez deux fois sur le composant Sort. Premièrement, vous avez besoin de définir l'état inversé avec un booléen. Le tri peut être inversé ou non inversé.

{title="src/App.js",lang=javascript}
~~~~~~~~
this.state = {
  results: null,
  searchKey: '',
  searchTerm: DEFAULT_QUERY,
  error: null,
  isLoading: false,
  sortKey: 'NONE',
# leanpub-start-insert
  isSortReverse: false,
# leanpub-end-insert
};
~~~~~~~~

Maintenant dans votre méthode de tri, vous pouvez évaluer si la liste est triée de façon inversée. C'est inversé si la `sortKey` dans l'état est la même que la `sortKey` entrante et que l'état inversé n'est pas déjà établi à true.

{title="src/App.js",lang=javascript}
~~~~~~~~
onSort(sortKey) {
# leanpub-start-insert
  const isSortReverse = this.state.sortKey === sortKey && !this.state.isSortReverse;
  this.setState({ sortKey, isSortReverse });
# leanpub-end-insert
}
~~~~~~~~

De nouveau vous pouvez passer la propriété reverse à votre composant Table.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error,
      isLoading,
      sortKey,
# leanpub-start-insert
      isSortReverse
# leanpub-end-insert
    } = this.state;

    ...

    return (
      <div className="page">
        ...
        <Table
          list={list}
          sortKey={sortKey}
# leanpub-start-insert
          isSortReverse={isSortReverse}
# leanpub-end-insert
          onSort={this.onSort}
          onDismiss={this.onDismiss}
        />
        ...
      </div>
    );
  }
}
~~~~~~~~

Maintenant le Table doit avoir un *block body* de fonction fléchée pour calculer les données.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
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
# leanpub-end-insert
    <div className="table">
      <div className="table-header">
        ...
      </div>
# leanpub-start-insert
      {reverseSortedList.map(item =>
# leanpub-end-insert
        ...
      )}
    </div>
# leanpub-start-insert
  );
}
# leanpub-end-insert
~~~~~~~~

Le tri inversé doit fonctionner maintenant.

Enfin mais pas des moindres, vous devez traiter une question ouverte pour l'intérêt d'une une expérience utilisateur améliorée. Un utilisateur peut-il distinguer quelle colonne est activement trié? Jusqu'à présent, ce n'est pas possible. Donnons un retour visuel à l'utilisateur.

Chaque composant Sort dispose déjà de sa `sortKey` spécifique. Elle pourrait être utilisée pour identifier le tri actif. Vous pouvez passer la `sortKey` depuis l'état interne du composant en tant que clé de tri actif pour votre composant Sort.

{title="src/App.js",lang=javascript}
~~~~~~~~
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
    <div className="table">
      <div className="table-header">
        <span style={{ width: '40%' }}>
          <Sort
            sortKey={'TITLE'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Title
          </Sort>
        </span>
        <span style={{ width: '30%' }}>
          <Sort
            sortKey={'AUTHOR'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Author
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          <Sort
            sortKey={'COMMENTS'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Comments
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          <Sort
            sortKey={'POINTS'}
            onSort={onSort}
# leanpub-start-insert
            activeSortKey={sortKey}
# leanpub-end-insert
          >
            Points
          </Sort>
        </span>
        <span style={{ width: '10%' }}>
          Archive
        </span>
      </div>
      {reverseSortedList.map(item =>
          ...
      )}
    </div>
  );
}
~~~~~~~~

Maintenant dans votre composant Sort, vous savez à l'aide de la `sortKey` et de `activeSortKey` si le tri est actif. Donnez à votre composant un attribut `className` supplémentaire, dans le cas où il est trié, pour donner à l'utilisateur un retour visuel.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Sort = ({
  sortKey,
  activeSortKey,
  onSort,
  children
}) => {
  const sortClass = ['button-inline'];

  if (sortKey === activeSortKey) {
    sortClass.push('button-active');
  }

  return (
    <Button
      onClick={() => onSort(sortKey)}
      className={sortClass.join(' ')}
    >
      {children}
    </Button>
  );
}
# leanpub-end-insert
~~~~~~~~

La manière de définir le `sortClass` est un peu malhabile, n'est ce pas? Il y a une petite bibliothèque sympa pour se débarrasser de çà. Premièrement vous devez l'installer.

{title="Command Line",lang="text"}
~~~~~~~~
npm install classnames
~~~~~~~~

Et deuxièmement vous devez l'importer tout en haut du fichier *src/App.js*.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import axios from 'axios';
import { sortBy } from 'lodash';
# leanpub-start-insert
import classNames from 'classnames';
# leanpub-end-insert
import './App.css';
~~~~~~~~

Maintenant vous pouvez l'utiliser pour définir votre `className` du composant avec des classes conditionnelles.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Sort = ({
  sortKey,
  activeSortKey,
  onSort,
  children
}) => {
# leanpub-start-insert
  const sortClass = classNames(
    'button-inline',
    { 'button-active': sortKey === activeSortKey }
  );
# leanpub-end-insert

  return (
# leanpub-start-insert
    <Button
      onClick={() => onSort(sortKey)}
      className={sortClass}
    >
# leanpub-end-insert
      {children}
    </Button>
  );
}
~~~~~~~~

De nouveau, lorsque vous lancez vos tests, vous devez voir des tests d'instantanés échouant mais aussi des tests unitaires échouant pour le composant Table. Dû au fait que vous avez changé une nouvelle fois vos représentations de composant, vous pouvez accepter les tests instantanés. Mais vous devez réparer le test unitaire. Dans votre fichier *src/App.test.js*, vous avez besoin de fournir une `sortKey` et le booléen `isSortReverse` pour le composant Table.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
...

describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
# leanpub-start-insert
    sortKey: 'TITLE',
    isSortReverse: false,
# leanpub-end-insert
  };

  ...

});
~~~~~~~~

De nouveau vous pourriez avoir besoin d'accepter les tests d'instantané échouant pour votre composant Table, car vous avez fourni des propriétés étendues pour le composant Table.

Enfin, votre interaction de tri avancé est maintenant terminée.

### Exercises:

* utiliser une bibliothèque comme [Font Awesome](http://fontawesome.io/) pour indiquer le tri (inversé)
  * cela pourrait être une icône de flèche vers le haut ou de flèche vers le bas à côté de chaque en-tête Sort
* lire plus à propos de la [bibliothèque classnames](https://github.com/JedWatson/classnames)

{pagebreak}

Vous avez appris les techniques de composant avancées dans React! Récapitulons les derniers chapitres :

* React
  * l'attribut res pour référencer les noeuds du DOM
  * les components d'ordre supérieur sont un moyen courant de construire des composants avancés
  * l'implementation d'interactions avancées dans React
  * les classNames conditionnels avec une élégante bibliothèque assistante
* ES6
  * la décomposition du reste pour séparer les objets et tableaux

Vous pouvez trouver le code source sur le [dépôt officiel](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.5).
