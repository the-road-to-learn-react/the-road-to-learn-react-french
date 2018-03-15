# Getting Real with an API

Maintenant il est temps d'obtenir quelques choses de concret avec une API, car il peut devenir très vite ennuyant de ne traiter qu'un échantillon de données.

Si vous n'êtes pas familier avec les APIs, Je vous encourage à [à lire mon périple où j'ai appris les APIs](https://www.robinwieruch.de/what-is-an-api-javascript/).

Connaissez-vous la plateforme [Hacker News](https://news.ycombinator.com/)? C'est un agrégateur de news génial à propos de sujets technologiques. Dans ce livre, vous utiliserez l'API d'Hacker News pour aller chercher les articles en tendance au sein de la plateforme. Il y a une API [basique](https://github.com/HackerNews/API) et une de [recherche](https://hn.algolia.com/api) pour obtenir les données de la plateforme. La dernière API à du sens dans le cadre de cette application dans le but de rechercher les histoires sur Hacker News. Vous pouvez visiter la spécification de l'API pour avoir une meilleure compréhension de la structure de données.

## Les méthodes du cycle de vie

Vous aurez besoin d'apprendre les méthodes de cycle de vie de React avant de commencer à aller chercher les données dans votre composant en utilisant une API. Ces méthodes sont un crochet à l'intérieur du cycle de vie d'un composant React. Ils peuvent être utilisés dans des composants de classe ES6, mais en aucun cas dans des composants fonctionnels stateless.

Vous rappelez vous quand dans un précédent chapitre vous avez appris à propos des classes ES6 et de comment ils sont utilisés dans React? Hormis la méthode `render()`, il y a plusieurs méthodes qui peuvent être surchargées dans un composant de classe React ES6. Toutes sont des méthodes du cycle de vie. Concentrons-nous sur ces derniers :

Vous connaissez dors et déjà deux méthodes du cycle de vie qui peuvent être utilisées dans le composant de classe ES6 : `constructor()` and `render()`.

Le constructeur est appelé seulement une seule fois quand une instance du composant est créée et insérée dans le DOM. Le composant est instancié. Ce procédé est appelé le montage du composant (*mounting of the component*).

La méthode `render()` est aussi appelée durant le procédé de montage, mais aussi lors de la mise à jour du composant. Chaque fois que l'état ou les propriétés du composant changent, la méthode `render()` du composant est appelée.

Maintenant vous en savez plus à propos des méthodes du cycle de vie et de quand elles sont appelées. D'ailleurs, vous les utilisez d'ores et déjà. Mais il y en a encore plus parmi eux.

Le montage d'un composant a deux méthodes de cycle de vie supplémentaires : `componentWillMount()` et `componentDidMount()`. Le constructeur est appelé en premier, `componentWillMount()` sera appelée avant la méthode `render()` et `componentDidMount()` est apellée après la méthode `render()`.

Dans l'ensemble le processus de montage a 4 méthodes de cycle de vie. Elles sont invoquées dans l'ordre suivant : 

* constructor()
* componentWillMount()
* render()
* componentDidMount()

Mais qu'advient-il du cycle de vie de la mise à jour d'un composant qui se produit lorsque l'état ou les propriétés changent? Dans l'ensemble il a 5 méthodes de cycle de vie dans l'ordre suivant : 

* componentWillReceiveProps()
* shouldComponentUpdate()
* componentWillUpdate()
* render()
* componentDidUpdate()

Dernier point, mais pas des moindres, il y a le cycle de vie de démontage. Il a seulement une méthode de cycle de vie : `componentWillUnmount()`.

Après tous, vous n'avez pas besoin de savoir toutes ces méthodes de cycle pour débuter. Cela peut-être encore un peu intimidant vous ne les utiliserez pas toutes. Même dans une grande application React vous utiliserez seulement une petite partie d'entre eux à l'exception du `constructor()` et de la méthode `render()`. Pourtant, il est bon de connaître que chaque méthode de cycle de vie peut être utilisé pour des cas d'utilisation spécifiques : 


* **constructor(props)** - C'est appelé lorsque le composant est initialisé. Vous pouvez donner un état initial au composant et lier les méthodes de classe dans cette méthode du cycle de vie.

* **componentWillMount()** - C'est appelé avant la méthode `render()` du cycle de vie. C'est pourquoi il peut être utilisé pour définir l'état interne du composant, car il ne déclenchera pas un second rendu du composant. Généralement c'est recommandé d'utiliser le `constructor()` pour mettre en place l'état initial.

* **render()** - Cette méthode du cycle de vie est obligatoire et retourne les éléments en tant que sortie du composant. La méthode devra être pure et par conséquent ne devra pas modifier l'état du composant. Il a une entrée comme propriétés et état et retourne un élément.

* **componentDidMount()** - Elle est appelée une seule fois lorsque le composant a été monté. C'est le moment parfait pour faire des requêtes asynchrones dans le but d'aller chercher des données depuis une API. Les données rapportées seront stockées dans l'état interne du composant pour être affichées dans la méthode du cycle de vie `render()`.

* **componentWillReceiveProps(nextProps)** - La méthode de cycle de vie est appelée durant le cycle de vie de mise à jour. En entrée vous obtenez les prochaines propriétés. Vous pouvez faire une différence entre les prochaines propriétés avec les propriétés précédentes, en utilisant `this.props`, pour appliquer un comportement différent basé sur la différence. De plus, vous pouvez établir l'état basé sur les prochaines propriétés.

* **shouldComponentUpdate(nextProps, nextState)** - C'est aussi appelée lors de la mise à jour du composant à cause d'un changement du state ou des propriétés. Vous l'utiliserez dans des applications React mûrs pour des optimisations de performance. Dépendant d'un boolean que vous retournez depuis cette méthode du cycle de vie, le composant et tous ses enfants seront rendus ou pas durant un cycle de vie de mise à jour. Vous pouvez empêcher la méthode de rendu du cycle de vie d'un composant.

* **componentWillUpdate(nextProps, nextState)** - La méthode de cycle de vie est immédiatement invoqué avant la méthode `render()`. Vous avez toujours les prochaines propriétés et le prochain état à votre disposition. Vous pouvez utiliser la méthode en dernier recours pour performer des préparatifs avant que la méthode de rendu soit éxecutée. Noter que vous ne pouvez plus déclencher `setState()`. Si vous voulez calculer l'état en fonction des prochaines propriétés, vous êtes obligé d'utiliser `componentWillReceiveProps()`.

* **componentDidUpdate(prevProps, prevState)**  La méthode de cycle de vie est immédiatement invoquée après la méthode `render()`. Vous pouvez l'utiliser comme opportunité de performer des opérations du DOM ou de performer des requêtes asychrones supplémentaires.

* **componentWillUnmount()** - Elle est appelée avant que vous détruisez votre composant. Vous devez utiliser cette méthode du cycle de vie pour performer toutes tâches de nettoyage.

Vous utilisez déjà les méthodes du cycle de vie `constructor()` et `render()`. Ce sont les méthodes du cycle de vie communément utilisées pour des composants de classe ES6. En fait, la méthode `render()` est requise, autrement vous ne seriez pas en mesure de retourner une instance de composant.

Il y a plus de méthode du cycle de vie : `componentDidCatch(error, info)`. Cela a été introduit dans [React 16] (https://www.robinwieruch.de/what-is-new-in-react-16/) et est utilisée pour attraper les erreurs dans le composant. Par exemple, afficher la liste d'échantillon de votre application fonctionne bien. Mais cela pourrait y avoir un cas lorsque la liste dans l'état local est établie à `null` par accident (ex: lors de la recherche d'une liste depuis une API externe, mais que la requête échoue et que vous écrivez l'état local de la liste à null). Après quoi, il ne serait plus possible de filtrer et mapper la liste, car c'est `null` et non une liste vide. Le composant sera cassé et l'entièreté de l'application échouera. Maintenant, en utilisant `componentDidCatch()`, vous pouvez attraper l'erreur, la stocker dans votre état local, et montrer un message optionnel à votre utilisateur de l'application qu'une erreur c'est produite.

### Exercices :

* lire plus à propos [ des méthodes du cycle de vie dans React](https://facebook.github.io/react/docs/react-component.html)
* lire plus à propos [de l'état relié aux méthodes du cycle de vie dans React](https://facebook.github.io/react/docs/state-and-lifecycle.html)
* lire plus à propos [de la gestion d'erreur dans les composants](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)

## Aller chercher les données

Maintenant vous êtes prêt pour aller chercher les données de l'API d'Hacker News. Il y a une méthode du cycle de vie mentionné qui peut être utilisée pour aller chercher les données `componentDidMount()`. Vous utiliserez l'API native *fetch* de JavaScript pour performer la requête.

Avant que nous puissions l'utiliser, établissons les constantes d'URL et les paramètres par défaut pour décomposer la requête d'API en morceaux.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

# leanpub-start-insert
const DEFAULT_QUERY = 'redux';

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
# leanpub-end-insert

...
~~~~~~~~

En JavaScript ES6, vous pouvez utiliser les [template strings](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals) pour concaténer les chaînes de caractères. Vous l'utiliserez pour concaténer votre URL pour le point d'entrée de l'API.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES6
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${DEFAULT_QUERY}`;

// ES5
var url = PATH_BASE + PATH_SEARCH + '?' + PARAM_SEARCH + DEFAULT_QUERY;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux
~~~~~~~~

Cela conservera votre composition d'URL souple pour le futur.

Mais passons à la requête de l'API où vous utiliserez l'URL. Le processus de recherche sera présenté en une fois, mais chaque étape sera expliquée par la suite.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
# leanpub-start-insert
      result: null,
      searchTerm: DEFAULT_QUERY,
# leanpub-end-insert
    };

# leanpub-start-insert
    this.setSearchTopStories = this.setSearchTopStories.bind(this);
# leanpub-end-insert
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  setSearchTopStories(result) {
    this.setState({ result });
  }

  componentDidMount() {
    const { searchTerm } = this.state;

    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(error => error);
  }

# leanpub-end-insert

  ...
}
~~~~~~~~

Beaucoup de choses dans ce code. Je pensais le casser en plusieurs petits morceaux. Mais encore une fois il serait difficile de saisir les relations de chacun des morceaux. Laissez-moi expliquer chaque étape en détail.

Premièrement, vous pouvez supprimer la liste d'objets, car vous retournez une liste réelle depuis l'API d'Hacker News. Les données d'échantillon ne sont plus utilisées maintenant. L'état initial de votre composant possède un résultat vide tout comme le terme de recherche par défaut. Le même terme de recherche par défaut est utilisé dans le champ d'entrée du composant Search et dans votre première requête.

Deuxièmement, vous utilisez la méthode du cycle de vie `componentDidMount()` pour aller chercher les données après que le composant est été monté. Dans le tout premier *fetch*, le terme de recherche par défaut issue de l'état local est utilisé. Il ira chercher les sujets reliés à "redux", car c'est le paramètre par défaut.

Troisièmement, l'API native *fetch* est utilisée. Les *templates strings* du JavaScript ES6 permettent de composer l'URL avec `searchTerm`. L'URL est l'argument pour la fonction d'API native *fetch*. La réponse a besoin d'être transformée en structure de données JSON, qui est une étape obligatoire pour une fonction native *fetch* lorsque l'on traite avec des structures de données JSON, et peut finalement être écrit comme résultat dans l'état interne du composant. De plus, le *catch block* est utilisé dans le cas d'une erreur. Si une erreur se produit durant la requête, la fonction lancera l'intérieur du *catch block* au lieu du *then block*. Dans un prochain chapitre du livre, vous inclurez la gestion d'erreur.

Dernier point, mais pas des moindres, n'oubliez pas de lier votre nouvelle méthode de composant dans le constructeur.

Maintenant vous pouvez utiliser les données obtenues à la place de la liste d'objets. Cependant, vous devez être de nouveau prudent. Le résultat n'est pas seulement une liste de données. [C'est un objet complexe avec des metas informations et une liste de succès qui sont dans notre cas les articles](https://hn.algolia.com/api). Vous pouvez afficher l'état interne avec un `console.log(this.state);` dans votre méthode `render()` pour le visualiser.

Dans la prochaine étape, vous utiliserez le résultat pour le rendu. Mais nous empêcherons de rendre n'importe quoi, ainsi nous retournerons null, lorsqu'il n'y a pas de résultat en premier lieu. Une fois que la requête de l'API est parvenue, le résultat est sauvegardé dans l'état et le composant App sera rerendu avec l'état mis à jour.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const { searchTerm, result } = this.state;

    if (!result) { return null; }

# leanpub-end-insert
    return (
      <div className="page">
        ...
        <Table
# leanpub-start-insert
          list={result.hits}
# leanpub-end-insert
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

Récapitulons ce qu'ils c'est passé durant le cycle de vie du composant. Votre composant a été initialisé par le constructeur. Après cela, il est rendu pour la première fois. Mais vous l'empêchez d'afficher n'importe quoi, car le résultat dans l'état local est null. C'est permis de retourner null pour un composant dans le but de ne rien afficher. Alors la méthode de cycle de vie `componentDidMount()` se lance. Dans cette méthode vous allez chercher de façon asynchrone les données depuis l'API d'Hacker News. Une fois les données arrivées, il change votre état interne du composant dans `setSearchTopStories()`. Après quoi, le cycle de vie de mis à jour entre en jeu car l'état local a été mis à jour. Le composant lance de nouveau la méthode `render()`, mais cette fois avec un résultat  hydraté dans votre état interne du composant. Le composant et ainsi le composant Table avec son contenu seront rendus.

Vous avez utilisé l'API native *fetch* qui est supporté par la plupart des navigateurs pour performer une requête asynchrone vers une API. La configuration de *create-react-app* fait en sorte qu'elle soit supportée au sein de tous les navigateurs. Il y a des packages node tiers que vous pouvez utiliser pour substituer l'API native *fetch* : [superagent](https://github.com/visionmedia/superagent) et [axios](https://github.com/mzabriskie/axios).

Garder en mémoire que le livre se construit sur la notation abrégée de JavaScript pour des vérifications sûres. Dans l'exemple précédent, le `if (!result)` a été utilisé en faveur de `if (result === null)`. C'est de même pour les autres cas au travers du livre. Par exemple, `if (!list.length)` est utilisé en faveur de `if (list.length === 0)` ou `if (someString)` est utilisé en faveur de `if (someString !== '')`. Lisez à propos de ce sujet si vous n'êtes pas assez à l'aise avec.

Retour à votre application : la liste des succès devrait être visible maintenant. Cependant, il y a également deux bugs régressifs dans l'application. Premièrement, le bouton "Dismiss" est cassé. Il ne connaît plus l'objet résultat complexe et opère encore sur la simple liste de l'état local lors du rejet d'un objet. Deuxièmement, lorsque la liste est affichée mais que vous essayez de chercher quelque chose d'autres, la liste reste filtrée côté client même si la recherche initiale a été faite en cherchant les articles côté serveur. Le comportement parfait serait d'aller chercher un autre objet résultat depuis l'API lors de l'utilisation du composant Search. Les deux bugs de régression seront fixés dans le prochain chapitre.

### Exercices :


* lire plus à propos des [ES6 template strings](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Template_literals)
* lire plus à propos de [l'API fetch native](https://developer.mozilla.org/en/docs/Web/API/Fetch_API)
* lire plus à propos [de la recherhce de données au sein de React](https://www.robinwieruch.de/react-fetching-data/)

## ES6 Spread Operators

Le bouton "Dismiss" ne fonctionne plus à cause de la méthode `onDismiss()` qui n'est pas consciente de l'objet résultat complexe. Il connaît seulement la liste simple dans l'état interne. Mais ce n'est plus une liste simple maintenant. Changeons cela pour opérer sur l'objet résultat au lieu de la liste elle-même.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
# leanpub-start-insert
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
    ...
  });
# leanpub-end-insert
}
~~~~~~~~

Mais que se passe-t-il maintenant dans `setState()`? Malheureusement le résultat est un objet complexe. La liste des hits est seulement une des multiples propriétés à l'intérieur de l'objet. Cependant, seule la liste sera mis à jour, quand un objet sera supprimé dans l'objet résultat, tandis que les autres propriétés restent les mêmes.

Une approche pourrait être d'écrire les hits dans l'objet résultat. Nous ne souhaitons pas le faire de cette façon, je le démontrerai.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ne pas faire çà
this.state.result.hits = updatedHits;
~~~~~~~~

React adopte les structures de données immutables. Ansi vous ne devez pas muter un objet (ou muter l'état directement). Une meilleure approche est de générer un nouvel objet basé sur les informations que vous possédez? Ainsi aucun des objets sera altéré. Vous conserverez les structures de données immutables. Vous retournerez toujours un nouvel objet et altérerez jamais un objet.

Par conséquent vous pouvez utiliser `Object.assign()` de l'ES6 JavaScript. Il prend en premier argument un objet cible. Tous les arguments suivants sont des objets sources. Ces objets sont fusionnés à l'intérieur de l'objet cible. L'objet cible peut être un objet vide. Il adopte l'immutabilité, aucun objet source sera muté. Cela ressemblera à cela : 

{title="Code Playground",lang="javascript"}
~~~~~~~~
const updatedHits = { hits: updatedHits };
const updatedResult = Object.assign({}, this.state.result, updatedHits);
~~~~~~~~

Les derniers objets surpasseront les premiers objets fusionnés lorsqu'ils partagent les mêmes noms de propriétés. Maintenant, appliquons à la méthode `onDismiss()` :

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
# leanpub-start-insert
    result: Object.assign({}, this.state.result, { hits: updatedHits })
# leanpub-end-insert
  });
}
~~~~~~~~

Ce pourrait déjà être la solution. Mais il y a un moyen plus simple en ES6 JavaScript ainsi que dans les futurs sortis JavaScript. Puis je introduire le *spread operator* pour vous? Il est constitué de seulement trois points : `...`. Quand il est utilisé, toutes les valeurs d'un tableau ou d'un objet seront copiées dans un autre tableau ou objet.

Examinons même si vous n'en avez apas encore besoin.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userList = ['Robin', 'Andrew', 'Dan'];
const additionalUser = 'Jordan';
const allUsers = [ ...userList, additionalUser ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

La variable `allUsers` est un tableau complètement nouveau. Les autres variables `userList` et `additionalUser` restent les mêmes. Vous pouvez même fusionner deux tableaux de cette façon en un nouveau tableau.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const oldUsers = ['Robin', 'Andrew'];
const newUsers = ['Dan', 'Jordan'];
const allUsers = [ ...oldUsers, ...newUsers ];

console.log(allUsers);
// output: ['Robin', 'Andrew', 'Dan', 'Jordan']
~~~~~~~~

Maintenant regardons l'*object spread operator*. C'est n'est pas de l'ES6 JavaScript. C'est encore une [proposition pour une prochaine version JavaScript](https://github.com/sebmarkbage/ecmascript-rest-spread) utilisé par la communauté React. C'est pourquoi *create-react-app* a incorporé la fonctionnalité dans la configuration.

Gloablement c'est idem qu'un *array spread operator* d'ES6 JavaScript mais avec des objets. Il copie chaque paire clé valeur à l'intérieur d'un nouvel objet.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const age = 28;
const user = { ...userNames, age };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

Plusieurs objets peuvent être propagés comme dans l'exemple de l'*array spread*.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const userNames = { firstname: 'Robin', lastname: 'Wieruch' };
const userAge = { age: 28 };
const user = { ...userNames, ...userAge };

console.log(user);
// output: { firstname: 'Robin', lastname: 'Wieruch', age: 28 }
~~~~~~~~

Après tout, il peut être utilisé pour remplacer `Object.assign()`.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedHits = this.state.result.hits.filter(isNotId);
  this.setState({
# leanpub-start-insert
    result: { ...this.state.result, hits: updatedHits }
# leanpub-end-insert
  });
}
~~~~~~~~

Maintenant le bouton "Dismiss" devra fonctionner de nouveau, car la méthode `onDismiss()` est consciente de l'objet résultat complexe et comment le mettre à jour après le rejet d'un objet depuis la liste.

### Exercices :

* lire plus à propos de l'[ES6 Object.assign()](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)
* lire plus à propos de l'[ES6 array spread operator](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Spread_operator)
  * le *object spread operator* est brièvement mentionné

## Rendu conditionnel

Le rendu conditionnel a été introduit assez tôt dans les applications React. Mais pas dans le cadre de ce livre, car il n'y avait pas encore de cas d'utilisation. Le rendu conditionnel se produit quand on souhaite faire une décision pour rendre soit un élément soit un autre élément. Parfois cela signifie rendre un élément ou rien. Après tout, l'usage le plus trivial d'un rendu conditionnel peut être représenté par une déclaration *if-else* en JSX.

Au démarrage l'objet `result` dans l'état interne du composant est `null`. Jusqu'à présent, le composant App retournait aucun élément quand le `result` ne provennait pas de l'API. C'est déjà un rendu conditionnel, car pour une certaine condition vous retournez le résultat plus tôt au sein de la méthode `render()` du cycle de vie. Le composant App rend soit ses éléments soit rien.

Nous pouvons aller plus loin. Cela a plus de sens d'englober le composant Table, qui est le seul composant qui dépend du `result`, dans un rendu conditionnel indépendant. Tout le reste devrait être affiché, même s'il n'y a pas encore de `result`. Vous pouvez simplement utiliser une opération ternaire dans votre JSX.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const { searchTerm, result } = this.state;
# leanpub-end-insert
    return (
      <div className="page">
        <div className="interactions">
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
          >
            Search
          </Search>
        </div>
# leanpub-start-insert
        { result
          ? <Table
            list={result.hits}
            pattern={searchTerm}
            onDismiss={this.onDismiss}
          />
          : null
        }
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

C'est une seconde option pour représenter un rendu conditionnel. Une troisième option est un opérateur `&&` logique. En JavaScript `true && 'Hello World'` est toujours évalué à 'Hello World'. Un `false && 'Hello World'` est toujours évalué à faux.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const result = true && 'Hello World';
console.log(result);
// output: Hello World

const result = false && 'Hello World';
console.log(result);
// output: false
~~~~~~~~

Dans React vous pouvez rendre utilise ce comportement. Si la condition est vraie, l'expression après l'opérateur `&&` logique sera la sortie. Si la condition est fausse, React l'ignore et saute l'expression. C'est applicable dans le cas du rendu conditionnel de Table, car il devrait retourner un Table ou rien.

{title="src/App.js",lang=javascript}
~~~~~~~~
{ result &&
  <Table
    list={result.hits}
    pattern={searchTerm}
    onDismiss={this.onDismiss}
  />
}
~~~~~~~~

C'étaient quelques approches pour utiliser le rendu conditionnel dans React. Vous pouvez lire à propos de [plus d'alternatives dans une liste exhaustive d'exemples pour des approches de rendu conditionnel](https://www.robinwieruch.de/conditional-rendering-react/). De plus, vous serez amené à connaitre leur différent cas d'utilisation et quand les appliquer.

En fin de compte, vous devrez être capable de voir les données récupérées dans votre application. Tout hormis le composant Table est affiché en attendant les données récupérées. Une fois la requête résolue le résultat est stocké à l'intérieur de l'état local, le Table est affiché car la méthode `render()` se lance de nouveau et la condition dans le rendu conditionnel se résout en faveur de l'affichage du composant Table.

### Exercices :

* lire plus à propos du [rendu conditionnel de React](https://facebook.github.io/react/docs/conditional-rendering.html)
* lire plus à propos des [différentes façons pour les rendus conditionnels](https://www.robinwieruch.de/conditional-rendering-react/)

## Recherche côté client ou côté serveur

Actuellement, lorsque vous utilisez le composant Search avec ses champs d'entrée, vous filtrerez la liste. Cependant cela se produit côté client. Maintenant vous allez utiliser l'API d'Hacker News pour rechercher côté serveur. Sans quoi vous traiterez uniquement la première réponse de l'API que vous obtiendriez via `componentDidMount()` avec en paramètre de terme de recherche celui par défaut.

Vous pouvez définir une méthode `onSearchSubmit()` dans votre composant App qui va chercher les résultats depuis l'API d'Hacker News lors de l'exécution d'une recherche dans le composant Search.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      result: null,
      searchTerm: DEFAULT_QUERY,
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
# leanpub-start-insert
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
# leanpub-end-insert
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...

# leanpub-start-insert
  onSearchSubmit() {
    const { searchTerm } = this.state;
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

La méthode `onSearchSubmit()` doit utiliser la même fonctionnalité que la méthode du cycle de vie `componentDidMount()`, mais cette fois-ci avec un terme de recherche modifiée issue de l'état local et non du terme de recherche par défaut. Ainsi vous pouvez extraire la fonctionnalité en tant que méthode de classe réutilisable.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      result: null,
      searchTerm: DEFAULT_QUERY,
    };

    this.setSearchTopStories = this.setSearchTopStories.bind(this);
# leanpub-start-insert
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
# leanpub-end-insert
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...

# leanpub-start-insert
  fetchSearchTopStories(searchTerm) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(error => error);
  }
# leanpub-end-insert

  componentDidMount() {
    const { searchTerm } = this.state;
# leanpub-start-insert
    this.fetchSearchTopStories(searchTerm);
# leanpub-end-insert
  }

  ...

  onSearchSubmit() {
    const { searchTerm } = this.state;
# leanpub-start-insert
    this.fetchSearchTopStories(searchTerm);
# leanpub-end-insert
  }

  ...
}
~~~~~~~~

Maintenant le composant de Search doit ajouter un bouton supplémentaire. Le bouton doit déclencher explicitement la requête de recherche. Autrement vous irez constamment chercher les données depuis l'API d'Hacker News lorsque votre champ d'entrée change. À part si vous voulez le faire explicitement dans un handler `onClick()`.

En alternative vous pouvez (retarder) *debounce* la fonction `onChange()` et économiser le bouton, mais cela ajouterait plus de complexité pour l'heure et peut-être ne serait pas l'effet escompté. Restons simple sans un *debounce* pour le moment.

Premièrement, passer la méthode `onSearchSubmit()` pour votre composant Search.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
    return (
      <div className="page">
        <div className="interactions">
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
# leanpub-start-insert
            onSubmit={this.onSearchSubmit}
# leanpub-end-insert
          >
            Search
          </Search>
        </div>
        { result &&
          <Table
            list={result.hits}
            pattern={searchTerm}
            onDismiss={this.onDismiss}
          />
        }
      </div>
    );
  }
}
~~~~~~~~

Deuxièmement, introduire un bouton dans votre composant Search. Le bouton possède le `type="submit"` et le formulaire utilise son attribut pour passer la méthode `onSubmit()`. Vous pouvez réutiliser la propriété enfant, mais cette fois elle sera utilisée pour le contenu du bouton.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Search = ({
  value,
  onChange,
  onSubmit,
  children
}) =>
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
# leanpub-end-insert
~~~~~~~~

Dans le Table, vous pouvez supprimer la fonctionnalité de filtre, car il ne sera plus filtré côté client (search). N'oubliez pas de supprimer la fonction `isSearched()` également. Elle ne sera plus utilisée. Maintenant, le résultat arrive directement de l'API d'Hacker News après que vous ayez cliqué sur le bouton "Search".

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
    return (
      <div className="page">
        ...
        { result &&
          <Table
# leanpub-start-insert
            list={result.hits}
            onDismiss={this.onDismiss}
# leanpub-end-insert
          />
        }
      </div>
    );
  }
}

...

# leanpub-start-insert
const Table = ({ list, onDismiss }) =>
# leanpub-end-insert
  <div className="table">
# leanpub-start-insert
    {list.map(item =>
# leanpub-end-insert
      ...
    )}
  </div>
~~~~~~~~

Lorsque vous essayez de rechercher, vous remarquerez que le navigateur se recharge. C'est un comportement natif du navigateur pour une callback de soumission dans un formulaire HTML. Dans React vous serez souvent amené au travers de la méthode d'évènement `preventDefault()` de supprimer le comportement natif du navigateur.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
onSearchSubmit(event) {
# leanpub-end-insert
  const { searchTerm } = this.state;
  this.fetchSearchTopStories(searchTerm);
# leanpub-start-insert
  event.preventDefault();
# leanpub-end-insert
}
~~~~~~~~

Maintenant vous devez être capable de rechercher différents sujets d'Hacker News. Parfait, vous interagissez avec une une véritable API. Il ne devrait plus avoir de recherche côté client dorénavant.

### Exercices :

* lire plus à propos [ des évènements synthétiques dans React](https://facebook.github.io/react/docs/events.html)
* expérimenter avec l'[API d'Hacker News](https://hn.algolia.com/api)

## Recherche paginée

Avez-vous une meilleure vision de la structure de données retournée? L'[Hacker News API](https://hn.algolia.com/api) retourne plus qu'une liste de hits. Précisément elle retourne une liste paginée. La propriété page, qui est `0` dans la première réponse, peut être utilisé pour aller chercher encore plus de sous-listes paginées en tant que résultat. Vous avez seulement besoin de transmettre la prochaine page avec le même terme de recherche auprès de l'API.

Étendons les constantes d'API composable ainsi il peut traiter les données paginées.

{title="src/App.js",lang=javascript}
~~~~~~~~
const DEFAULT_QUERY = 'redux';

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
# leanpub-start-insert
const PARAM_PAGE = 'page=';
# leanpub-end-insert
~~~~~~~~

Maintenant vous pouvez utiliser la nouvelle constante pour ajouter la paramètre page pour votre requête d'API.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const url = `${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}`;

console.log(url);
// output: https://hn.algolia.com/api/v1/search?query=redux&page=
~~~~~~~~

La méthode `fetchSearchTopStories()` prendra la page en tant que second argument. Si vous ne fournissez pas de second argument, il se repliera sur la page `0` pour la requête initiale. Ainsi les méthodes `componentDidMount()` et `onSearchSubmit()` iront chercher la première page de la première requête. Toutes recherches additionnelles devraient aller chercher la prochaine page en fournissant le deuxième argument.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

# leanpub-start-insert
  fetchSearchTopStories(searchTerm, page = 0) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}`)
# leanpub-end-insert
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
      .catch(error => error);
  }

  ...

}
~~~~~~~~

L'argument de page utilise la valeur par défaut des arguments ES6 pour introduire une solution de repli vers la page `0` au cas où aucun argument de page est fourni à la fonction.

Maintenant vous pouvez utiliser la page courante de la réponse d'API dans `fetchSearchTopStories()`. Vous pouvez utiliser cette méthode dans un bouton pour aller chercher plus de sujets au handler `onClick` du bouton. Utilisons le Button pour aller chercher plus de données paginées depuis l'API d'Hacker News. Vous aurez seulement besoin de définir l'handler `onClick()` qui prend le terme de recherche courant et la prochaine page (current page + 1).

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, result } = this.state;
# leanpub-start-insert
    const page = (result && result.page) || 0;
# leanpub-end-insert
    return (
      <div className="page">
        <div className="interactions">
        ...
        { result &&
          <Table
            list={result.hits}
            onDismiss={this.onDismiss}
          />
        }
# leanpub-start-insert
        <div className="interactions">
          <Button onClick={() => this.fetchSearchTopStories(searchTerm, page + 1)}>
            More
          </Button>
        </div>
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

En plus, dans votre méthode `render()` vous devrez vous assurer par défaut d'aller chercher à la page 0 lorsqu'il n'y a pas encore de résultat. Souvenez-vous de cela la méthode `render()` est appelée avant que les données sont rapportées de façon asynchrone dans la méthode du cycle de vie `componentDidMount()`.

Il manque une étape. Vous allez chercher la prochaine page de données, mais il écartera votre précédente page de données. Il serait idéal de concaténer l'ancienne avec la nouvelle liste de hits au sein de l'état local et du nouvel objet résultat. Ajustons la fonctionnalité pour ajouter les nouvelles données plutôt que de les écraser.

{title="src/App.js",lang=javascript}
~~~~~~~~
setSearchTopStories(result) {
# leanpub-start-insert
  const { hits, page } = result;

  const oldHits = page !== 0
    ? this.state.result.hits
    : [];

  const updatedHits = [
    ...oldHits,
    ...hits
  ];

  this.setState({
    result: { hits: updatedHits, page }
  });
# leanpub-end-insert
}
~~~~~~~~

Maintenant plusieurs choses se produisent dans la méthode `setSearchTopStories()`. Premièrement, vous obtenez les hits et page depuis le résultat.

Deuxièmement, vous avez vérifié s'il y a déjà des vieux hits. Quand la page est à 0, c'est une nouvelle requête de recherche issue de `componentDidMount()` ou `onSearchSubmit()`. Les hits sont vides. Mais quand vous cliquez sur le bouton "More" pour aller chercher les données paginées la page n'est plus 0. C'est une nouvelle page. Les anciens hits sont déjà stockés dans votre état et peuvent ainsi être utilisés.

Troisièmement, vous ne voulez pas écarter les anciens hits. Vous pouvez fusionner les anciens et nouveaux hits issus de la dernière requête API. La fusion des deux listes peut être effectuée avec le *spead operator* de JavaScript ES6.

Quatrièmement, vous écrivez les hits fusionnés et la page courante dans l'état local du composant.

Vous pouvez faire un dernier ajustement. Quand vous essayez le bouton "More" il va seulement chercher quelques objets de la liste. L'URL de l'API peut être étendu pour aller chercher davantage d'objets de la liste avec chaque requête. De nouveau, vous pouvez ajouter plus de constantes d'URL composable.

{title="src/App.js",lang=javascript}
~~~~~~~~
const DEFAULT_QUERY = 'redux';
# leanpub-start-insert
const DEFAULT_HPP = '100';
# leanpub-end-insert

const PATH_BASE = 'https://hn.algolia.com/api/v1';
const PATH_SEARCH = '/search';
const PARAM_SEARCH = 'query=';
const PARAM_PAGE = 'page=';
# leanpub-start-insert
const PARAM_HPP = 'hitsPerPage=';
# leanpub-end-insert
~~~~~~~~

Now you can use the constants to extend the API URL.

{title="src/App.js",lang=javascript}
~~~~~~~~
fetchSearchTopStories(searchTerm, page = 0) {
# leanpub-start-insert
  fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
# leanpub-end-insert
    .then(response => response.json())
    .then(result => this.setSearchTopStories(result))
    .catch(error => error);
}
~~~~~~~~

Après çà, la requête auprès de l'API d'Hacker News va chercher plus d'éléments de la liste que précédemment en une seule requête. Comme vous pouvez le voir, une API puissante telle que l'API d'Hacker News vous donne plein de façons pour expérimenter des données du monde réel. Vous devez vous efforcer de faire usage de cela lors de l'apprentissage de quelque chose de nouveau et de plus excitant. Ici [comment j'ai appris le pouvoir que les APIs procurent](https://www.robinwieruch.de/what-is-an-api-javascript/) lors de l'apprentissage d'un nouveau langage de programmation ou d'une bibliothèque.

### Exercices :

* lire plus à propos des [valeurs par défaut des arguments ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters)
* expérimenter avec les [paramètres de l'API d'Hacker News](https://hn.algolia.com/api)

## Cache client

Chaque recherche soumise fait une requête à l'API d'Hacker News. Vous pourriez rechercher "redux", suivi par "react" et finalement de nouveau "redux". Au total cela fait 3 requêtes. Mais vous recherchez "redux" deux fois et les deux fois prennent un aller-retour asynchrone complet pour aller chercher les données. Dans un cache côté client vous souhaiterez stocker chaque résultat. Quand une requête auprès de l'API est effectuée, il vérifie si le résultat est déjà présent. S'il l'est, le cache est utilisé. Autrement la requête API est faite pour aller chercher les données.

Dans le but d'obtenir un cache client pour chaque requête, vous avez à stocker plusieurs `results` plutôt qu'un seul `result` dans votre état interne du composant. L'objet `results` sera une *map* avec le terme de recherche en tant que clé et le résultat en tant que valeur. Chaque résultat depuis l'API sera sauvegardé par terme de recherche (key).

Pour le moment, votre résultat dans l'état local ressemble au suivant :

{title="Code Playground",lang="javascript"}
~~~~~~~~
result: {
  hits: [ ... ],
  page: 2,
}
~~~~~~~~

Imaginez vous devoir faire deux requêtes API. Une pour le terme "redux" et une autre pour "react". Les objets résultats devront ressembler au suivant : 

{title="Code Playground",lang="javascript"}
~~~~~~~~
results: {
  redux: {
    hits: [ ... ],
    page: 2,
  },
  react: {
    hits: [ ... ],
    page: 1,
  },
  ...
}
~~~~~~~~

Implémentons le cache côté client avec React `setState()`. Premièrement, renommer l'objet `result` pour `results` dans l'état initial du composant. Deuxièmement, définissez un `searchKey` temporaire qui est utilisé pour stocker chaque `result`.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
# leanpub-start-insert
      results: null,
      searchKey: '',
# leanpub-end-insert
      searchTerm: DEFAULT_QUERY,
    };

    ...

  }

  ...

}
~~~~~~~~

La `searchKey` doit être établie avant que chaque requête soit effectuée. Elle reflète le `searchTerm`. Vous pouvez vous demander : Pourquoi ne pas utiliser le `searchTerm` en premier lieu? C'est un point crucial à comprendre avant de continuer avec l'implémentation. Le `searchTerm` est une variable changeante, car il sera changé chaque fois vous tapez dans le champ d'entrée de Search. Cependant, à la fin vous aurez besoin de variable stable. Cela détermine le terme de recherche récemment soumis auprès de l'API et il peut être utilisé pour retrouver le résultat correct depuis la *map* de results. C'est un pointeur vers notre résultat courant dans le cache et ainsi il peut être utilisé pour afficher le résultat courant dans votre méthode `render()`.

{title="src/App.js",lang=javascript}
~~~~~~~~
componentDidMount() {
  const { searchTerm } = this.state;
# leanpub-start-insert
  this.setState({ searchKey: searchTerm });
# leanpub-end-insert
  this.fetchSearchTopStories(searchTerm);
}

onSearchSubmit(event) {
  const { searchTerm } = this.state;
# leanpub-start-insert
  this.setState({ searchKey: searchTerm });
# leanpub-end-insert
  this.fetchSearchTopStories(searchTerm);
  event.preventDefault();
}
~~~~~~~~

Maintenant vous devez ajuster la fonctionnalité où le résultat est stocké dans l'état interne du composant. Il devrait stocker chaque résultat par `searchKey`.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  setSearchTopStories(result) {
    const { hits, page } = result;
# leanpub-start-insert
    const { searchKey, results } = this.state;

    const oldHits = results && results[searchKey]
      ? results[searchKey].hits
      : [];
# leanpub-end-insert

    const updatedHits = [
      ...oldHits,
      ...hits
    ];

    this.setState({
# leanpub-start-insert
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      }
# leanpub-end-insert
    });
  }

  ...

}
~~~~~~~~

La `searchKey` sera utilisée en tant que clé pour sauvegarder les hits mis à jour et la page dans la *map* `results`.

Premièrement, vous devez retrouver la `searchKey` depuis l'état du composant. Souvenez-vous que la `searchKey` sera établie au `componentDidMount()` et au `onSearchSubmit()`.

Deuxièmement, comme avant les anciens hits doivent être fusionnés avec les nouveaux hits. Mais cette fois les anciens hits sont récupérés depuis la *map* `results` avec la `searchKey` en tant que clé.

Troisièmement, un nouveau résultat peut être écrit dans la *map* `results` dans l'état. Examinons l'objet `results` dans `setState()`.

{title="src/App.js",lang=javascript}
~~~~~~~~
results: {
  ...results,
  [searchKey]: { hits: updatedHits, page }
}
~~~~~~~~

La dernière partie fait en sorte de stocker le résultat mis à jour par `searchKey` dans la *map* `results`. La valeur est un objet avec des hits et une propriété page. La `searchKey` est le terme de recherche. Vous avez déjà appris la syntaxe `[searchKey]: ...`. C'est un nom de propriété ES6 calculé. Cela vous aide à allouer des valeurs dynamiquement dans un objet.

La partie du haut a besoin de propager tous les autres résultats via `searchKey` dans l'état en utilisant l'*object spread operator*. Autrement vous perdrez tous les résultats que vous avec avez stockés précédemment.

Maintenant vous stockez tous les résultats par termes de recherche. C'est la première étape pour rendre disponible votre cache. Dans la prochaine étape, vous pouvez retrouver le résultat en fonction de la `searchKey` non changeante depuis votre *map* de `results`. C'est pourquoi vous devez introduire la `searchKey` en premier lieu comme variable non changeante. Autrement la récupération pourrait être rompue lorsque vous souhaiterez utiliser le `searchTerm` changeant pour retrouver le résultat courant, car cette valeur pourrait changer lorsque vous souhaiterez utiliser le composant Search.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
# leanpub-start-insert
    const {
      searchTerm,
      results,
      searchKey
    } = this.state;

    const page = (
      results &&
      results[searchKey] &&
      results[searchKey].page
    ) || 0;

    const list = (
      results &&
      results[searchKey] &&
      results[searchKey].hits
    ) || [];

# leanpub-end-insert
    return (
      <div className="page">
        <div className="interactions">
          ...
        </div>
# leanpub-start-insert
        <Table
          list={list}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
        <div className="interactions">
# leanpub-start-insert
          <Button onClick={() => this.fetchSearchTopStories(searchKey, page + 1)}>
# leanpub-end-insert
            More
          </Button>
        </div>
      </div>
    );
  }
}
~~~~~~~~

Puisque vous retournez par défaut une liste vide quand il n'y a pas de résultat via `searchKey`, vous pouvez maintenant économiser le rendu conditionnel pour le composant Table. De plus vous aurez besoin de passer la `searchKey` plutôt que le `searchTerm` auprès du bouton "More". Sinon, votre recherche paginée dépend de la valeur `searchTerm` qui est changeante. De plus soyez sûr de conserver la propriété `searchTerm` changeante pour le champ d'entrée du composant "Search".

La fonctionnalité de recherche doit fonctionner de nouveau. Il stocke tous les résultats depuis l'API Hacker News.

De plus, la méthode `onDismiss()` a besoin d'être améliorée. Elle traite encore l'objet `result`. Maintenant elle doit traiter les multiples `results`.

{title="src/App.js",lang=javascript}
~~~~~~~~
  onDismiss(id) {
# leanpub-start-insert
    const { searchKey, results } = this.state;
    const { hits, page } = results[searchKey];
# leanpub-end-insert

    const isNotId = item => item.objectID !== id;
# leanpub-start-insert
    const updatedHits = hits.filter(isNotId);

    this.setState({
      results: {
        ...results,
        [searchKey]: { hits: updatedHits, page }
      }
    });
# leanpub-end-insert
  }
~~~~~~~~

Le bouton "Dismiss" doit fonctionner de nouveau.

Cependant, rien n'arrête l'application d'envoyer une requête API à chaque soumission de requête. Même s'il y aurait déjà un résultat, il n'y a aucun contrôle pour empêcher la requête. Ainsi la fonctionnalité de cache n'est pas encore achevée. Elle cache les résultats, mais elle ne les utilise pas. La dernière étape serait d'empêcher la requête d'API lorsqu'un résultat est disponible en cache.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {

    ...

# leanpub-start-insert
    this.needsToSearchTopStories = this.needsToSearchTopStories.bind(this);
# leanpub-end-insert
    this.setSearchTopStories = this.setSearchTopStories.bind(this);
    this.fetchSearchTopStories = this.fetchSearchTopStories.bind(this);
    this.onSearchChange = this.onSearchChange.bind(this);
    this.onSearchSubmit = this.onSearchSubmit.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  needsToSearchTopStories(searchTerm) {
    return !this.state.results[searchTerm];
  }
# leanpub-end-insert

  ...

  onSearchSubmit(event) {
    const { searchTerm } = this.state;
    this.setState({ searchKey: searchTerm });
# leanpub-start-insert

    if (this.needsToSearchTopStories(searchTerm)) {
      this.fetchSearchTopStories(searchTerm);
    }

# leanpub-end-insert
    event.preventDefault();
  }

  ...

}
~~~~~~~~

Maintenant votre client fait une requête auprès de l'API seulement une seule fois même si vous recherchez un terme de recherche une seconde fois. Même les données paginées avec plusieurs pages seront cachées de cette façon, car vous sauvegardez toujours la dernière page pour chaque résultat dans la *map* `results`. N'est-ce pas une approche percutante pour introduire un cache pour votre application? L'API d'Hacker News vous fournit tout ce dont vous avez besoin pour mettre en cache les données paginées de façon effective.

## Gestion des erreurs

Tout est place concernant vos interactions avec l'API d'Hacker News. Vous avez même introduit une façon élégante de mettre en cache vos résultats depuis l'API et d'employer sa fonctionnalité de liste paginée pour aller chercher une liste sans fin de sous-listes d'articles. Mais il y a une pièce manquante. Malheureusement, c'est souvent oublié lors de développements d'applications de nos jours : la gestion des erreurs. C'est tellement facile d'implémenter le cas avec succès sans se soucier des erreurs pouvant se produire tout du long.

Dans ce chapitre, vous introduirez une solution efficace pour ajouter une gestion des erreurs pour votre application en cas de requête API erronée. Vous avez déjà pris connaissance de la nécessité  des *building blocks* en React pour introduire la gestion d'erreur : l'état local et le rendu conditionnel. Globalement, l'erreur est seulement un autre état dans React. Lorsqu'une erreur se produit, vous le stockerez dans l'état local et l'afficherez avec un rendu conditionnel au sein de votre composant. C'est tout. Implémentons cela dans le composant App, car c'est le composant qui est utilisé en premier lieu pour aller chercher les données depuis l'API d'Hacker News. Tout d'abord, vous devez introduire l'erreur dans l'état local. C'est inutilisé à null, mais défini avec un *error object* en cas d'erreur.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      results: null,
      searchKey: '',
      searchTerm: DEFAULT_QUERY,
# leanpub-start-insert
      error: null,
# leanpub-end-insert
    };

    ...
  }

...

}
~~~~~~~~

Deuxièmement, vous pouvez utiliser le *catch block* dans votre *fetch* natif pour stocker l'*error object* dans l'état local en utilisant `setState()`. À chaque fois, que la requête d'API n'est pas un succès, le *catch block* sera exécuté.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  fetchSearchTopStories(searchTerm, page = 0) {
    fetch(`${PATH_BASE}${PATH_SEARCH}?${PARAM_SEARCH}${searchTerm}&${PARAM_PAGE}${page}&${PARAM_HPP}${DEFAULT_HPP}`)
      .then(response => response.json())
      .then(result => this.setSearchTopStories(result))
# leanpub-start-insert
      .catch(error => this.setState({ error }));
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

Troisièmement, vous pouvez retrouver l'*error object* depuis votre état local dans la méthode `render()` et afficher un message en cas d'erreur en utilisant le rendu conditionnel de React.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
# leanpub-start-insert
      error
# leanpub-end-insert
    } = this.state;

    ...

# leanpub-start-insert
    if (error) {
      return <p>Something went wrong.</p>;
    }
# leanpub-end-insert

    return (
      <div className="page">
        ...
      </div>
    );
  }
}
~~~~~~~~

C'est tout. Si vous voulez tester que votre gestion d'erreur fonctionne, vous pouvez substituer l'URL de l'API par quelque chose d'inexistant.

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.foo.bar.com/api/v1';
~~~~~~~~

Après quoi, vous devrez obtenir le message d'erreur au lieu de votre application. Vous pouvez choisir où vous souhaitez placer le rendu conditionnel pour votre message d'erreur. Dans le cas présent, l'entièreté de l'application ne sera plus du tout affichée. Ce n'est surement pas la meilleure expérience utilisateur. Donc pourquoi ne pas afficher soit le composant Table ou le message d'erreur? Le reste de l'application sera encore visible en cas d'erreur.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const {
      searchTerm,
      results,
      searchKey,
      error
    } = this.state;

    const page = (
      results &&
      results[searchKey] &&
      results[searchKey].page
    ) || 0;

    const list = (
      results &&
      results[searchKey] &&
      results[searchKey].hits
    ) || [];

    return (
      <div className="page">
        <div className="interactions">
          ...
        </div>
# leanpub-start-insert
        { error
          ? <div className="interactions">
            <p>Something went wrong.</p>
          </div>
          : <Table
            list={list}
            onDismiss={this.onDismiss}
          />
        }
# leanpub-end-insert
        ...
      </div>
    );
  }
}
~~~~~~~~

À la fin, n'oubliez pas de remettre l'URL d'API existante.

{title="src/App.js",lang=javascript}
~~~~~~~~
const PATH_BASE = 'https://hn.algolia.com/api/v1';
~~~~~~~~

Votre application devrait encore fonctionner, mais cette fois avec une gestion d'erreur en cas de requête d'API qui échoue.

### Exercices :

* lire plus à propos de la [gestion des erreurs pour les composants React](https://reactjs.org/blog/2017/07/26/error-handling-in-react-16.html)
* lire plus à propos de [pourquoi les frameworks sont importants](https://www.robinwieruch.de/why-frameworks-matter/)

{pagebreak}

Vous avez appris à interagir avec l'API React! Récapitulons les derniers chapitres :


* React
  * Les méthodes du cycle de vie du composant de classe ES6 pour différent cas d'utilisation
  * componentDidMount() pour les interactions d'API
  * rendus conditionnels
  * évènement synthétique concernant les formulaires
  * gestion d'erreur
* ES6
  * template strings pour composer de chaines de caractères
  * spread operator pour des structures de données immutables.
  * noms de prorpiétés calculés
* Général
  * interaction avec l'API d'Hacker News
  * API native du navigateur fetch
  * recherche côté client et côté serveur
  * pagination des données
  * cache côté client

Encore une fois il y a un intérêt à faire une pause. Intérioriser les acquis et les appliquer par vous-même. Vous pouvez expérimenter avec le code source que vous avez écrit jusqu'ici.

Vous pouvez trouver le code source dans le [dépôt officiel](https://github.com/rwieruch/hackernews-client/tree/4.3).
