# Les bases de React

Ce chapitre vous guidera au travers des bases de React. Il couvre l'état et les interactions au sein des composants, car les composants statiques sont un peu moroses, n'est ce pas? De plus, vous apprendrez les différentes façons de déclarer un composant et comment conserver les composants composables et réutilisables. Soyez prêt à faire vivre vos composants.

## État interne de composant

L'état interne du composant, aussi connus comme état local, permet de sauvegarder, modifier et supprimer des propriétés qui sont stockées dans votre composant. Le composant de classe ES6 peut utiliser un constructeur pour initialiser l'état interne du composant nous le verrons ultérieurement. Le constructeur est appelé seulement une seule fois quand le composant est initialisé.

Introduisons le constructeur de classe.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

# leanpub-start-insert
  constructor(props) {
    super(props);
  }
# leanpub-end-insert

  ...

}
~~~~~~~~

Lorsque nous avons un constructeur dans notre composant de classe ES6, il est obligatoire d'appeler `super();` parce que le composant App est une sous-classe de `Component`. D'où,  l'`extends Component` dans notre déclaration du composant App. Vous apprendrez ultérieurement plus à propos des composants de classe ES6.

Vous pouvez appeler `super(props);` aussi. Cela établi `this.props` dans notre constructeur si vous désirez y accéder depuis le constructeur. Autrement, quand on accède `this.props` dans votre constructeur, il serait `undefined`. Vous en apprendrez plus sur les propriétés de composant React ultérieurement.

Maintenant, dans votre cas, l'état initial dans votre composant devrait être un échantillon d'une liste d'éléments.

{title="src/App.js",lang=javascript}
~~~~~~~~
const list = [
  {
    title: 'React',
    url: 'https://facebook.github.io/react/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  ...
];

class App extends Component {

  constructor(props) {
    super(props);

# leanpub-start-insert
    this.state = {
      list: list,
    };
# leanpub-end-insert
  }

  ...

}
~~~~~~~~

L'état est lié à la classe via l'objet `this`. Ainsi, vous pouvez accéder à l'état local dans tout votre composant. Par exemple, il peut être utilisé dans la méthode `render()`. Précédemment, vous avez établi une liste d'éléments dans votre méthode `render()` qui était défini à l'extérieur de votre composant. Maintenant vous êtes à même d'utiliser la liste à partir de votre état local à l'intérieur de votre composant.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {this.state.list.map(item =>
# leanpub-end-insert
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

La liste fait partie de votre composant maintenant. Elle y réside dans l'état interne du composant. Vous pouvez ajouter, modifier ou supprimer des éléments à l'intérieur et depuis votre liste. Chaque fois vous changez votre état de composant, la méthode `render()` de votre composant sera éxécuté de nouveau. C'est comme cela que vous pouvez simplement modifier votre état interne de composant et être sûre que le composant rerendra et affichera les données correctes qui sont issues de l'état local.

Mais soyez vigilant. N'établissez pas l'état directement. Vous aurez la nécessité d'appeler la méthode nommée `setState()` pour modifier votre état. Vous apprendrez à la connaître dans le prochain chapitre.

### Exercices :

* expérimenter l'état local
  * définir plus de données pour l'état initial à l'intérieur du constructeur
  * utiliser et accéder à l'état dans votre méthode `render()`
* lire plus à propos du [constructeur de classe ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes#Constructor)

## ES6 Object Initializer

En JavaScript ES6, vous pouvez utiliser la syntaxe d'abréviation de propriété pour initialiser vos objets plus concisément. Imaginez l'initialisation de l'objet suivant : 

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name: name,
};
~~~~~~~~

Quand le nom de la propriété de votre objet est le même que vote nom de variable, vous pouvez faire ceci : 

{title="Code Playground",lang="javascript"}
~~~~~~~~
const name = 'Robin';

const user = {
  name,
};
~~~~~~~~

Vous pouvez faire de même, dans votre application. Le nom de la variable liste et le nom de la propriété de l'état partagent le même nom.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
this.state = {
  list: list,
};

// ES6
this.state = {
  list,
};
~~~~~~~~

Un autre élégant *helper* est les noms de méthodes abrégés. En JavaScript ES6, vous pouvez initialiser les méthodes dans un objet plus concisément.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var userService = {
  getUserName: function (user) {
    return user.firstname + ' ' + user.lastname;
  },
};

// ES6
const userService = {
  getUserName(user) {
    return user.firstname + ' ' + user.lastname;
  },
};
~~~~~~~~

Enfin mais non moins important, il vous est permis d'utiliser les noms de propriétés interprétées en JavaScript ES6.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var user = {
  name: 'Robin',
};

// ES6
const key = 'name';
const user = {
  [key]: 'Robin',
};
~~~~~~~~

Peut-être les noms de propriétés interprétées n'ont pas encore de sens. Pourquoi devriez vous avoir besoin de cela? Dans un futur chapitre, vous arriverez à un point où vous pourrez l'utiliser pour allouer des valeurs par clé de façon dynamique dans un objet. C'est propre pour générer des tables de recherche en JavaScript.

### Exercices :

* expérimenter avec l'initialiseur d'objet ES6
* lire à propos de [l'initialiseur d'objet ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer)


## Flux de données unidirectionnel

Maintenant vous avez quelques états internes dans votre composant App. Cependant, vous n'avez pas encore manipulé l'état local. L'état est statique et ainsi le composant l'est tout autant. Un bon moyen pour expérimenter la manipulation d'état est d'avoir quelques interactions de composant.

Ajoutons un bouton pour chaque élément dans la liste affichée. Le bouton écrira "Dismiss" et supprimera l'élément de la liste. Cela pourrait être utile finalement lorsque vous souhaitez seulement conserver une liste d'éléments non lus et de rejeter les éléments dont vous n'êtes pas intéressés.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
# leanpub-start-insert
            <span>
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
            </span>
# leanpub-end-insert
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

La méthode de classe `onDismiss()` n'est pas encore définie. Nous le ferons dans un moment, mais pour l'heure concentrons-nous sur l'*handler* `onClick` de l'élément bouton. Comme vous pouvez le voir, la méthode `onDismiss()` dans l'*handler* `onClick` est comprise dans une autre fonction. C'est une fonction fléchée. Ainsi, vous pouvez vous glisser dans la propriété `objectID` de l'objet `item` pour identifier l'objet que vous supprimerez. Une alternative serait de définir la fonction à l'extérieur de l'*handler* `onClick` et seulement passer la fonction définie à l'*handler*. Un prochain chapitre abordera le sujet des *handlers* dans les éléments avec plus en détail.

Avez-vous remarqué les multiples lignes pour l'élément bouton? Noter que les éléments avec plusieurs attributs peuvent être assez en désordre sur une seule ligne à certains endroits. C'est pourquoi l'élément bouton est utilisé sur plusieurs lignes et des indentations pour garder de la lisibilité. Mais ce n'est pas obligatoire. C'est seulement une recommandation de style de code dont je recommande chaudement.

Maintenant vous devez implémenter la fonction `onDismiss()`. Elle prend un id pour identifier l'élément à supprimer. La fonction est liée à la classe et ainsi devient une méthode de classe. C'est pourquoi vous y accédez avec `this.onDismiss()` et non `onDismiss()`. L'objet `this` est votre instance de classe. Dans le but de définir l'`onDismiss()` comme méthode de classe, vous devez la lier dans le constructeur. Les liens seront expliqués plsu tard dans un autre chapitre.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

# leanpub-start-insert
    this.onDismiss = this.onDismiss.bind(this);
# leanpub-end-insert
  }

  render() {
    ...
  }
}
~~~~~~~~

Dans la prochaine étape, vous devrez définir ses fonctionnalités, la logique métier, au sein de votre classe. Les méthodes de classe peuvent être définies de la façon suivante.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  onDismiss(id) {
    ...
  }
# leanpub-end-insert

  render() {
    ...
  }
}
~~~~~~~~

Maintenant vous êtes capable de définir ce qui se produit à l'intérieur de la méthode de classe. Globalement vous souhaitez supprimer de la liste l'élément identifié par l'id et stocker une liste mise à jour pour votre état local. Après quoi, la liste mise à jour sera utilisé pour relancer la méthode `render()` afin de l'afficher. L'élément retiré ne doit plus apparaître dès lors.

Vous pouvez retirer un élément de la liste en utilisant la fonctionnalité JavaScript natif *filter*. La fonction *filter* prend une fonction en entrée. La fonction a accès à chaque valeur de la liste, parce qu'elle itère par-dessus la liste. De cette façon, vous pouvez évaluer chaque élément dans la liste basée sur la condition de filtre. Si l'évaluation pour un élément est true, l'élément reste dans la liste. Autrement elle sera filtrée de la liste. De plus, il est bon de savoir que la fonction retourne une nouvelle liste et ne mute pas l'ancienne liste. Cela respecte la convention de React d'avoir des structures de données immutables.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(function isNotId(item) {
    return item.objectID !== id;
  });
# leanpub-end-insert
}
~~~~~~~~

Dans la prochaine étape, vous pouvez extraire la fonction et la passer dans la fonction de filtre.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  function isNotId(item) {
    return item.objectID !== id;
  }

  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

De plus, vous pouvez faire plus concis en utilisant de nouveau la fonction fléchée de JavaScript ES6?

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-end-insert
}
~~~~~~~~

Vous pourrez même le faire en ligne de nouveau, comme vous avez fait avec l'*handler* `onClick` du bouton, mais cela pourrait devenir moins lisible.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
# leanpub-start-insert
  const updatedList = this.state.list.filter(item => item.objectID !== id);
# leanpub-end-insert
}
~~~~~~~~

Maintenant, la liste supprime l'élément cliqué. Cependant l'état n'est pas encore mis à jour. Par conséquent vous pouvez finalement utiliser la méthode de classe `setState()` pour mettre à jour la liste dans l'état interne du composant.

{title="src/App.js",lang=javascript}
~~~~~~~~
onDismiss(id) {
  const isNotId = item => item.objectID !== id;
  const updatedList = this.state.list.filter(isNotId);
# leanpub-start-insert
  this.setState({ list: updatedList });
# leanpub-end-insert
}
~~~~~~~~

Maintenant lancer une nouvelle fois l'application et essayait le bouton "Dismiss". Il devrait fonctionner. Ce que vous avez expérimenté est l'**unidirectional data flow** au sein de React. Vous déclenchez une action dans votre vue avec `onCLick()`, une fonction ou méthode de classe modifie l'état interne du composant et la méthode `render()` du composant est lancée une nouvelle fois pour updater la vue.

![Mise à jour de l'état interne à l'aide du flux de données unidirectionnel](images/set-state-to-render-unidirectional.png)

### Exercices :

* lire plus à propos [de l'état et du cycle de vie de React](https://facebook.github.io/react/docs/state-and-lifecycle.html)

## Bindings

C'est important d'apprendre le *bindings* de classes en JavaScript quand on utilise les composants de classe de React ES6. Dans le précédent chapitre, vous avez lié votre méthode de classe `onDismiss()` dans le constructeur.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  constructor(props) {
    super(props);

    this.state = {
      list,
    };

    this.onDismiss = this.onDismiss.bind(this);
  }

  ...
}
~~~~~~~~

Pourquoi vouloir faire cela en premier lieu? L'étape de binding est nécessaire, car les méthodes de classe ne sont pas automatiquement liées au `this` de l'instance de la classe. Démontrons cela à l'aide du composant de classe ES6 suivant.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Le composant est bien rendu, mais quand vous cliquez sur le bouton, vous obtiendrez `undefined` dans votre console log. C'est l'une des principales sources de bug quand on vient à utiliser React, car si vous souhaitez accéder à `this.state` dans votre méthode de classe, il ne peut être retrouvé car `this` est `undefined`. Donc dans le but de rendre accessible `this` dans vos méthodes de classe, vous devez lier les méthodes de classe avec `this`.

Dans la composant de classe suivante la méthode de classe est correctement lié dans le constructeur de la classe.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
# leanpub-start-insert
  constructor() {
    super();

    this.onClickMe = this.onClickMe.bind(this);
  }
# leanpub-end-insert

  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Lorsque vous essayez de nouveau le bouton, l'objet `this`, pour être plus précis l'instance de classe, devrait être définie et vous serez en mesure d'accéder à `this.state`, ou comme vous l'apprendrez plus tard `this.props`.

Le binding de la méthode de classe peut aussi apparaître ailleurs. Par exemple, elle peut apparaître dans la méthode de classe `render()`.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe() {
    console.log(this);
  }

  render() {
    return (
      <button
# leanpub-start-insert
        onClick={this.onClickMe.bind(this)}
# leanpub-end-insert
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Mais vous devriez éviter cela, car il liera la méthode de classe à chaque fois que la méthode `render()` est lancée. Globalement, il le lance à chaque fois que votre composant est mis à jour ce qui a des implications de performance. Lorsque l'on lie la méthode de classe dans le constructeur, vous la liez seulement une seule fois au commencement lorsque le composant est instancié. C'est la meilleure approche.

Un autre point, les développeurs parfois imagine définir la partie métier de leurs méthodes de classe à l'intérieur du constructeur.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

# leanpub-start-insert
    this.onClickMe = () => {
      console.log(this);
    }
# leanpub-end-insert
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Vous devriez l'éviter également, car cela va mettre le désordre dans votre constructeur avec le temps. Le constructeur est seulement ici pour instancier votre classe avec l'ensemble des propriétés. C'est pour cela que la partie logique métier des méthodes de classe ne devrait pas être définie à l'extérieur du constructeur.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  constructor() {
    super();

    this.doSomething = this.doSomething.bind(this);
    this.doSomethingElse = this.doSomethingElse.bind(this);
  }

  doSomething() {
    // faire quelque chose
  }

  doSomethingElse() {
    // faire autre chose
  }

  ...
}
~~~~~~~~

Enfin et surtout, il est utile de mentionner que les méthodes de classe peuvent être automatiquement autoliées sans le binding explicite en utilisant les fonctions fléchées JavaScript ES6.

{title="Code Playground",lang=javascript}
~~~~~~~~
class ExplainBindingsComponent extends Component {
  onClickMe = () => {
    console.log(this);
  }

  render() {
    return (
      <button
        onClick={this.onClickMe}
        type="button"
      >
        Click Me
      </button>
    );
  }
}
~~~~~~~~

Si le binding répétitif dans le constructeur vous ennuie, vous pouvez à la place y aller avec cette approche. La documentation officielle de React reste conformé à l'utilisation du binding de méthode de classe à l'intérieur du constructeur. C'est pour cela que ce livre le sera tout autant.

### Exercices :

* essayer les différentes approches de bindings et console log l'objet `this`

## Event Handler

Le chapitre devrez-vous procurer une compréhension plus global des *event handlers* dans les éléments. Dans votre application, vous allez utiliser l'élément bouton suivant pour retirer l'élément de la liste.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={() => this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

C'est déjà un cas d'utilisation complexe, car vous êtes obligé de passer une valeur à la méthode de classe et donc vous devez l'englober à l'intérieur d'une autre fonction (fléchée). Donc essentiellement, il a besoin d'être une fonction qui est passé à l'*event handler*. Le code suivant ne fonctionnera pas, parce que la méthode de classe devrez être executée immédiatement quand vous ouvrez l'application dans le navigateur.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Quand on utilise `onClick={doSomething()}`, la fonction `doSomething()` sera exécutée immédiatement lorsque vous ouvrez l'application dans le navigateur. L'expression dans l'*handler* est évaluée. Puisque la valeur retournée n'est plus une fonction, plus rien se produira lorsque vous cliquerez sur le bouton. Mais quand on utilise `onClick={doSomething}` alors que `doSomething` est une fonction, elle sera exécutée que lors du click sur le bouton. Le même principe s'applique pour la méthode de classe `onDismiss()` qui est utilisée dans votre application.

Cependant, l'utilisation d'`onClick={this.onDismiss}` n'est pas suffisant, car d'une manière ou d'une autre la propriété `item.objectID` a besoin d'être passé à la méthode de classe pour identifier l'élément qui sera sur le point d'être enlevé. C'est pourquoi elle doit être englobée à l'intérieur d'une autre fonction dans le but d'y glisser la propriété à l'intérieur. Le concept est appelé en JavaScript les fonctions d'ordre supérieur et sera expliqué brièvement plus tard.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
  onClick={() => this.onDismiss(item.objectID)}
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Une solution de contournement serait de définir la fonction englobante quelque part à l'extérieur et seulement transmettre la fonction définie dans l'*handler*. Puisqu'il a besoin d'accéder à un élément individuel, il doit être présent à l'intérieur du bloc de la fonction *map*.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item => {
# leanpub-start-insert
          const onHandleDismiss = () =>
            this.onDismiss(item.objectID);
# leanpub-end-insert

          return (
            <div key={item.objectID}>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
              <span>
                <button
# leanpub-start-insert
                  onClick={onHandleDismiss}
# leanpub-end-insert
                  type="button"
                >
                  Dismiss
                </button>
              </span>
            </div>
          );
        }
        )}
      </div>
    );
  }
}
~~~~~~~~

Après tout, il a besoin d'être une fonction qui est passé à l'*handler* de l'élément. En tant qu'exemple, essayer à la place ce code :

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
            ...
            <span>
              <button
# leanpub-start-insert
                onClick={console.log(item.objectID)}
# leanpub-end-insert
                type="button"
              >
                Dismiss
              </button>
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

Cela se lancera lorsque vous ouvrez l'application dans le navigateur mais pas lorsque vous cliquez sur le bouton. Tandis que le code suivant s'exécutera seulement lorsque vous cliquez sur le bouton. C'est une fonction qui est exécutée lorsque vous déclenchez l'*handler*.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
# leanpub-start-insert
  onClick={function () {
    console.log(item.objectID)
  }}
# leanpub-end-insert
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Dans le but de rester concis, vous pouvez la transformer de nouveau en une fonction fléchée de JavaScript ES6. C'est ce que nous faisons également avec la méthode de classe `onDismiss()`.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

<button
# leanpub-start-insert
  onClick={() => console.log(item.objectID)}
# leanpub-end-insert
  type="button"
>
  Dismiss
</button>

...
~~~~~~~~

Souvent les arrivants à React ont des difficultés au sujet de l'utilisation des fonctions au sein des *handlers* d'évènements. C'est pourquoi j'ai essayé de l'expliquer avec le plus de détails ici. En fin de compte, vous devez vous retrouver avec le code suivant dans votre bouton avec une fonction fléchée JavaScript ES6 concise qui a accès à la propriété `objectID` de l'objet `item`.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  ...

  render() {
    return (
      <div className="App">
        {this.state.list.map(item =>
          <div key={item.objectID}>
            ...
            <span>
# leanpub-start-insert
              <button
                onClick={() => this.onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
# leanpub-end-insert
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

Un autre sujet pertinent de performance, qui est souvent mentionné, sont les implications de l'utilisation des fonctions fléchées dans les *handlers* d'évènement. Par exemple, l'handler `onCLick` pour la méthode `onDismiss()` englobe la méthode dans une autre fonction fléchée pour être capable de transmettre l'identifieur d'objet. Donc à chaque fois que la méthode `render()` se lance, l'*handler* instancie une fonction fléchée d'ordre supérieur. Il *peut* avoir un impact sur la performance de votre application, mais la plupart du temps vous ne remarquerez rien. Imaginez-vous avez une importante table de données avec 1000 éléments et chaque ligne ou colonne a une telle fonction fléchée dans un *handler* d'évènement. Alors, cela vaut le coup de penser aux implications de performance et par conséquent vous devez implémenter un composant de Bouton dédié pour lier la méthode à l'intérieur du constructeur. Mais avant que cela se produise c'est une optimisation prématurée. Il y plus d'intérêt à se concentrer sur Rect lui-même.

### Exercices :

* essayer les différentes approches en utilisant les fonctions dans l'handler `onCLick` de votre bouton.

## Interactions avec les Forms et Events

Ajoutons une autre interaction pour l'application dans le but d'expérimenter les évènements dans React. L'interaction est une fonctionnalité de recherche. L'entrée du champ de recherche devrait être utilisée pour filtrer temporairement votre liste basée sur la propriété titre d'un élément.

Dans la première étape, vous allez définir un formulaire avec un champ d'entrée dans votre JSX.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
# leanpub-start-insert
        <form>
          <input type="text" />
        </form>
# leanpub-end-insert
        {this.state.list.map(item =>
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

Dans le scénario suivant vous allez taper à l'intérieur du champ d'entrée et filtrer temporairement la liste par le terme de recherche qui est utilisé dans le champ d'entrée. Pour être capable de filtrer la liste basée sur la valeur du champ d'entrée, vous avez besoin de stocker la valeur du champ d'entrée dans votre état local. Mais comment accèdez vous à la valeur? Vous pouvez utiliser **synthetic events** dans React pour accéder au contenu de l'évènement.

Définisssons un *handler* `onChange` pour le champ d'entrée.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
# leanpub-start-insert
          <input
            type="text"
            onChange={this.onSearchChange}
          />
# leanpub-end-insert
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~

La fonction est liée au composant de nouveau par une méthode de classe. Vous êtes dans l'obligation de lier et définir la méthode.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
    };

# leanpub-start-insert
    this.onSearchChange = this.onSearchChange.bind(this);
# leanpub-end-insert
    this.onDismiss = this.onDismiss.bind(this);
  }

# leanpub-start-insert
  onSearchChange() {
    ...
  }
# leanpub-end-insert

  ...
}
~~~~~~~~

Lors de l'utilisation d'un handler dans votre élément, vous avez accès à l'évènement synthétique de React dans votre signature de fonction callback.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

# leanpub-start-insert
  onSearchChange(event) {
# leanpub-end-insert
    ...
  }

  ...
}
~~~~~~~~

L'évènement a la valeur du champ d'entrée dans son objet `target`. Ainsi vous serez en mesure de mettre à jour l'état local avec le terme de recherche de nouveau en utilisant `this.setState()`.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  onSearchChange(event) {
# leanpub-start-insert
    this.setState({ searchTerm: event.target.value });
# leanpub-end-insert
  }

  ...
}
~~~~~~~~

De plus, vous ne devez pas oublier de définir l'état initial pour la propriété `searchTerm` dans le constructeur. Le champ d'entrée devrait être vide au démarrage et ainsi la valeur devrait être une chaine de caractères vide.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  constructor(props) {
    super(props);

    this.state = {
      list,
# leanpub-start-insert
      searchTerm: '',
# leanpub-end-insert
    };

    this.onSearchChange = this.onSearchChange.bind(this);
    this.onDismiss = this.onDismiss.bind(this);
  }

  ...
}
~~~~~~~~

Maintenant vous stockez la valeur de l'entrée vers votre état interne du composant chaque fois que la valeur dans le champ d'entrée change.

Un aparté à propos de la mise à jour de l'état local dans un composant React. Il serait envisageable de supposer que lors de la mise à jour du `searchTerm` avec `this.setState()` la liste a besoin d'être passée également pour la conserver. Mais ce n'est pas le cas. `this.setState()` de React est une fusion superficielle. Il conserve les propriétés soeurs dans l'objet état quand on met à jour une seule propriété à l'intérieur. Ainsi l'état de la liste, même si vous avez déjà rejeté un objet de celle-ci, restera la même lors de la mise à jour de la propriété `searchTerm`.

Retournons à votre application. La liste n'est pas encore filtrée en fonction de la valeur du champ d'entrée qui est stocké dans l'état local. Simplement vous devez filtrer temporairement la liste basée sur le `searchTerm`. Vous disposez de tous ce dont vous avez besoin pour filtrer. Donc comment maintenant la filtrer de façon temporaire?
Dans votre méthode `render()`, avant votre *map* par-dessus la liste, vous pouvez appliquer un filtre par-dessus. Le filtre devra seulement évaluer si le `searchTerm` correspond avec la propriété *title* de l'objet. Vous avez déjà utilisé la fonctionnalité de filtre du JavaScript natif, donc faisons-le de nouveau. Vous pouvez glisser la fonction de filtre avant la fonction *map*, car la fonction de filtre retourne un nouveau tableau et ainsi la fonction *map* peut l'utiliser c'est une façon très commode.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
# leanpub-start-insert
        {this.state.list.filter(...).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

Abordons la fonction *filter* cette fois-ci de manière différente. Nous souhaitons définir l'argument de filtre, à la fonction qui est passé à la fonction *filter*, depuis l'extérieur du composant de classe ES6. Là nous n'avons pas accès à l'état du composant et ainsi nous n'avons aucun accès à la propriété `searchTerm` pour évaluer la condition de filtre. Nous devons transmettre le `searchTerm` à la fonction de filtre et nous devons retourner une nouvelle fonction, pour évaluer la condition. Ceci est appelé une fonction d'ordre supérieur.

Normalement je ne souhaite pas mentionner les fonctions d'ordre supérieur, mais dans un livre React cela fait totalement sens. Cela fait sens de connaitre les fonctions d'ordre supérieur, car React gère un concept de composants d'ordre supérieur. Vous ferez connaissance de ce concept plus tard dans le livre. Maintenant, concentrons-nous sur la fonctionnalité de filtre.

Premièrement, vous devez définir la fonction d'ordre supérieur à l'extérieur de votre composant App.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function isSearched(searchTerm) {
  return function(item) {
    // certaines conditions qui retournent true ou false
  }
}
# leanpub-end-insert

class App extends Component {

  ...

}
~~~~~~~~

La fonction prend le `searchTerm` et retourne une autre fonction, car après tout, la fonction *filter* prend une fonction en entrée. La fonction retournée a accès à l'objet d'élément car c'est la fonction qui est passée auprès de la fonction *filter*. De plus, la fonction retournée sera utilisée pour filtrer la liste basée sur la condition définie dans la fonction. Définissons la condition.

{title="src/App.js",lang=javascript}
~~~~~~~~
function isSearched(searchTerm) {
  return function(item) {
# leanpub-start-insert
    return item.title.toLowerCase().includes(searchTerm.toLowerCase());
# leanpub-end-insert
  }
}

class App extends Component {

  ...

}
~~~~~~~~

La condition dit que vous associez le motif entrant du `searchTerm` avec la propriété *title* de l'élément de votre liste. Vous pouvez faire cela avec la fonctionnalité JavaScript intégrée `includes`. Seulement quand le motif correspond, vous retournez true et l'élément reste dans la liste. Lorsque le motif ne correspond pas l'élément est supprimé de la liste. Soyez attentif à la correspondance du motif : vous ne devez pas oublier de mettre en minuscule les deux chaînes de caractères. Autrement il y aura une non-correspondance entre le terme de recherche 'redux' et un titre d'élément 'Redux'. Puisque nous sommes en train de travailler sur une liste immutable et que nous retournons une nouvelle liste en utilisant la fonction *filter*, la liste originale dans l'état interne ne sera en aucun cas modifiée.

Il reste une chose à mentionner : Nous trichons un petit peu en utilisant la fonctionnalité JavaScript intégrée `includes`. C'est d'ores et déjà une fonctionnalité ES6. À quoi cela ressemblerait en JavaScript ES5? Vous devriez utiliser la fonction `indexOf()` pour obtenir l'index de l'élément dans la liste. Lorsque l'élément est dans la liste,`indexOf()`retournera son index du tableau.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
string.indexOf(pattern) !== -1

// ES6
string.includes(pattern)
~~~~~~~~

Une autre façon soignée de refactorer peut être réalisé de nouveau avec la fonction fléchée d'ES6. Cela rend la fonction plus concise :

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
function isSearched(searchTerm) {
  return function(item) {
    return item.title.toLowerCase().includes(searchTerm.toLowerCase());
  }
}

// ES6
const isSearched = searchTerm => item =>
  item.title.toLowerCase().includes(searchTerm.toLowerCase());
~~~~~~~~

Chacun défendra quelle fonction est la plus lisible. Personnellement je préfère la seconde. L'écosystème React utilise beaucoup de concept de programmation fonctionnelle. Il se produira souvent que vous utiliserez une fonction qui retourne des fonctions (fonction d'ordre supérieur). En JavaScript ES6, vous pouvez les exprimer de façon plus concise avec les fonctions fléchées.

Dernier point mais pas des moindres, vous êtes obligé d'utiliser la fonction `isSearched()` définie pour filtrer votre liste. Vous lui transmettez la propriété `searchTerm` de votre état local, elle retourne la fonction d'entrée du filtre, et filtre votre liste en se basant sur la condition de filtre. Après quoi, il dresse la liste filtrée par-dessus pour afficher un élément UI pour chacun des éléments de la liste.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    return (
      <div className="App">
        <form>
          <input
            type="text"
            onChange={this.onSearchChange}
          />
        </form>
# leanpub-start-insert
        {this.state.list.filter(isSearched(this.state.searchTerm)).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
  }
}
~~~~~~~~

La fonctionnalité de recherche devrait fonctionner maintenant. Essayer par vous-même dans le navigateur.

### Exercices :

* lire plus à propos des [évènements React](https://facebook.github.io/react/docs/handling-events.html)
* lire plus à propos [des fonctions d'ordre supérieur](https://en.wikipedia.org/wiki/Higher-order_function)

## ES6 Destructuring

Il y a une certaine façon en JavaScript ES6 pour facilité l'accès aux propriétés dans les objets et les tableaux. Cela s'appelle la décomposition. Comparez les bouts de code suivant en JavaScript ES5 et ES6.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const user = {
  firstname: 'Robin',
  lastname: 'Wieruch',
};

// ES5
var firstname = user.firstname;
var lastname = user.lastname;

console.log(firstname + ' ' + lastname);
// output: Robin Wieruch

// ES6
const { firstname, lastname } = user;

console.log(firstname + ' ' + lastname);
// output: Robin Wieruch
~~~~~~~~

Tandis que vous êtes obligé d'ajouter des lignes supplémentaires à chaque fois que vous voulez accéder à la propriété d'un objet en JavaScript ES5, vous pouvez le faire en une seule ligne en JavaScript ES6. Une bien meilleure pratique pour la lisibilité est l'utilisation de plusieurs lignes lorsque vous décomposez un objet en plusieurs propriétés.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const {
  firstname,
  lastname
} = user;
~~~~~~~~

Idem pour les tableaux. Vous pouvez tout autant les décomposer. Encore une fois, le multiligne conservera votre code plus lisible et visionnable.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const users = ['Robin', 'Andrew', 'Dan'];
const [
  userOne,
  userTwo,
  userThree
] = users;

console.log(userOne, userTwo, userThree);
// output: Robin Andrew Dan
~~~~~~~~

Peut-être avez-vous remarqué que l'objet de l'état local dans le composant App peut être décomposé de la même façon. Vous pouvez raccourcir la ligne de code du *filter* et de la *map*.

{title="src/App.js",lang=javascript}
~~~~~~~~
  render() {
# leanpub-start-insert
    const { searchTerm, list } = this.state;
# leanpub-end-insert
    return (
      <div className="App">
        ...
# leanpub-start-insert
        {list.filter(isSearched(searchTerm)).map(item =>
# leanpub-end-insert
          ...
        )}
      </div>
    );
~~~~~~~~

Vous pouvez le faire à la manière ES5 ou ES6 :

{title="Code Playground",lang="javascript"}
~~~~~~~~
// ES5
var searchTerm = this.state.searchTerm;
var list = this.state.list;

// ES6
const { searchTerm, list } = this.state;
~~~~~~~~

Mais puisque ce livre utilise JavaScript ES6 la plupart du temps, vous devrez vous y conformer.

### Exercices :

* lire plus à propos [décomposition ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

## Les composants contrôlés

Vous avez déjà appris des choses au sujet du flux de données unidirectionnel en React. Le même principe est appliqué pour les champs d'entrée, qui mettent à jour l'état local avec le `searchTerm` dans le but de filtrer la liste. Quand l'état change, la méthode `render()` se relance de nouveau et utilise le nouveau `searchTerm` à partir de l'état local pour appliquer la condition de filtre.

Mais n'avons-nous pas oublié quelque chose dans l'élément input? Une balise HTML arrive avec un attribut `value`. L'attibut `value` habituellement possède la valeur qui est affichée dans le champ d'entrée. Dans ce cas ce serait la propriété `searchTerm`. Cependant, il semble que nous en ayons pas besoin en React.

C'est faux. Les éléments de formulaire tels que `<input>`, `<textarea>` et `<select>` tiennent leur propre état en HTML pur. Ils modifient la valeur intérieurement une fois que quelqu'un la change depuis l'extérieur. Dans React ceci est appelé un **uncontrolled component**, car il gère son propre état. En React, vous devez être sûre de faire de ces éléments des **controlled components**.

Comment devez vous faire cela? Vous devez seulement définir la valeur de l'attribut du champ d'entrée. La valeur est déjà sauvegardé dans la propriété d'état `searchTerm`. Donc  pourquoi ne pas y accéder par là?

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
        <form>
          <input
            type="text"
# leanpub-start-insert
            value={searchTerm}
# leanpub-end-insert
            onChange={this.onSearchChange}
          />
        </form>
        ...
      </div>
    );
  }
}
~~~~~~~~

C'est tout. La boucle de flux de données unidirectionnel pour le champ d'entrée est auto-contenue dorénavant. L'état interne du composant est le seul et unique source de vérité pour le champ d'entrée.

La gestion totale de l'état interne et le flux de données unidirectionnel pourraient être nouveaux pour vous. Mais une fois vous l'aurez utilisé, il deviendra un flux naturel pour implémenter des choses en React. En général, React apporte un nouveau patron de conception avec le flux de données unidirectionnel dans le monde des *single page applications*. C'est d'ailleurs adopté par plusieurs frameworks et bibliothèques maintenant.

### Exercices :

* lire plus à propos des [formulaires React](https://facebook.github.io/react/docs/forms.html)

## Diviser les composants

Maintenant, vous avez un imposant composant App. Il continue de grandir et peut finalement devenir confus. Vous devez le diviser en plusieurs morceaux de composants plus petit.

Commençons par utiliser un composant pour l'entrée de recherche et un composant pour la liste d'éléments.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search />
        <Table />
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

Vous pouvez passer ces propriétés de composants pour qu'ils puissent fonctionner d'eux-mêmes. Dans le cas du composant App il a besoin de passer les propriétés gérées dans l'état local et ses méthodes de classe.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        />
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
# leanpub-end-insert
      </div>
    );
  }
}
~~~~~~~~

Maintenant vous pouvez définir les composants à côté de votre composant App. Ces composants seront également des composants de classe ES6. Ils rendent les mêmes éléments comme auparavant.

Le tout premier est le composant Search.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  ...
}

# leanpub-start-insert
class Search extends Component {
  render() {
    const { value, onChange } = this.props;
    return (
      <form>
        <input
          type="text"
          value={value}
          onChange={onChange}
        />
      </form>
    );
  }
}
# leanpub-end-insert
~~~~~~~~

Le second est le composant Table.

{title="src/App.js",lang=javascript}
~~~~~~~~
...

# leanpub-start-insert
class Table extends Component {
  render() {
    const { list, pattern, onDismiss } = this.props;
    return (
      <div>
        {list.filter(isSearched(pattern)).map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
              <button
                onClick={() => onDismiss(item.objectID)}
                type="button"
              >
                Dismiss
              </button>
            </span>
          </div>
        )}
      </div>
    );
  }
}
# leanpub-end-insert
~~~~~~~~

Maintenant vous avez trois composants de classe ES6. Peut-être avez-vous remarquez l'objet `props` qui est accessible via l'instance de classe en utilisant `this`. Les *props*, version raccourcie pour *properties*, possèdent toutes les valeurs que vous avez transmises au composant lorsque vous les utilisées dans votre composant App. De cette manière, les composants peuvent passer les propriétés vers le bas de l'arbre de composant.

En extractant ces composants du composant App, vous serez en mesure de les réutiliser ailleurs. Puisque les composants obtiennent leur valeurs en utilisant l'objet `props`, vous pouvez transmettre à tout moment les différentes propriétés à vos composants lorsque vous les utilisez ailleurs. Ces composants sont devenu réutilisables.

### Exercices :

* envisager les composants que vous pourrez diviser comme cela a été le cas de Search et Table
  * mais ne le faite pas maintenant, sinon, vous rencontrerez des conflits dans les prochains chapitres

## Composants composables

Il y a  une petite propriété qui est accessible dans l'objet `props` : la propriété `children`. Vous pouvez l'utiliser pour transmettre des éléments à vos composants depuis le dessus, qui sont inconnus pour le composant lui-même, mais rendant possible la composition de composant entre les uns et les autres. Regardons à quoi cela ressemble lorsque vous passez seulement un texte (string) en tant qu'enfant au composant Search.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
      <div className="App">
# leanpub-start-insert
        <Search
          value={searchTerm}
          onChange={this.onSearchChange}
        >
          Search
        </Search>
# leanpub-end-insert
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
      </div>
    );
  }
}
~~~~~~~~

Maintenant le composant Search peut décomposer la propriété `children` de l'objet `props`. Alors il peut spécifier où le `children` devrait être affiché.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Search extends Component {
  render() {
# leanpub-start-insert
    const { value, onChange, children } = this.props;
# leanpub-end-insert
    return (
      <form>
# leanpub-start-insert
        {children} <input
# leanpub-end-insert
          type="text"
          value={value}
          onChange={onChange}
        />
      </form>
    );
  }
}
~~~~~~~~

Maintenant, le texte "Search" peut être visible juste à côté de votre champ d'entrée. Lorsque vous utilisez le composant Search ailleurs, vous pouvez choisir un texte différent si vous le souhaitez. Après tout, vous pouvez transmettre autre chose que tu texte en tant que children. Vous pouvez transmettre un élément et des arbres d'éléments (qui peuvent être encapsulé par des composants de nouveau) en tant qu'enfant. La propriété children rend possible le tissage de composants entre eux.

### Exercices :

* lire plus à propos [du modèle de composition de React](https://facebook.github.io/react/docs/composition-vs-inheritance.html)

## Composants réutilisables

Les composants réutilisables et composables vous responsabilisent pour fournir des hiérarchies de composants compétents. Il y a la couche de vue fondatrice de React. Les derniers chapitres mentionnèrent le terme de réutilisabilité. Vous pouvez réutiliser les composants Table et Search dorénavant. Même le composant App est réutilisable, car vous pouvez l'instancier ailleurs de nouveau.

Définissons un composant encore plus réutilisable, un composant Button, qui sera finalement réutilisé plus souvent.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Button extends Component {
  render() {
    const {
      onClick,
      className,
      children,
    } = this.props;

    return (
      <button
        onClick={onClick}
        className={className}
        type="button"
      >
        {children}
      </button>
    );
  }
}
~~~~~~~~

Cela peut sembler redondant de déclarer un tel composant. Vous utiliserez un composant `Button` au lieu de l'élément `button`. Il vous épargne seulement le `type="button"`. Hormis pour le type de l'attribut vous avez à définir tout le reste lorsque vous voulez utiliser le composant Button. Mais vous êtes obligé de penser sur de l'investissement à long terme ici. Imaginez-vous avez plusieurs boutons dans votre application, mais vous désirez modifier un attribut, le style ou un comportement pour le bouton. Sans le composant vous devriez refactorer tous les boutons. Au lieu de cela le composant Button assure d'avoir seulement une seule source vérité. Un Button pour refactorer tous les boutons en une seule fois. Un Buton pour gouverner tous les autres.

Puisque vous avez déjà un élément bouton, vous pouvez utiliser le composant Button à la place. Il omet l'attribut type, car le composant Button le spécifie.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Table extends Component {
  render() {
    const { list, pattern, onDismiss } = this.props;
    return (
      <div>
        {list.filter(isSearched(pattern)).map(item =>
          <div key={item.objectID}>
            <span>
              <a href={item.url}>{item.title}</a>
            </span>
            <span>{item.author}</span>
            <span>{item.num_comments}</span>
            <span>{item.points}</span>
            <span>
# leanpub-start-insert
              <Button onClick={() => onDismiss(item.objectID)}>
                Dismiss
              </Button>
# leanpub-end-insert
            </span>
          </div>
        )}
      </div>
    );
  }
}
~~~~~~~~

Le composant Button attend une propriété `className` dans les propriétés. L'attribut `className` est un autre dérivé React pour l'attribut HTML `class`. Mais nous ne transmettons aucun `className` lorsque le Button était utilisé. Dans le code il devrait être plus explicite que la `className` dans le composant Button soit optionnelle.

Par conséquent, vous pouvez utiliser le paramètre par défaut qui est une fonctionnalité de JavaScript ES6.

{title="src/App.js",lang=javascript}
~~~~~~~~
class Button extends Component {
  render() {
    const {
      onClick,
# leanpub-start-insert
      className = '',
# leanpub-end-insert
      children,
    } = this.props;

    ...
  }
}
~~~~~~~~

Maintenant, quand il n'y a pas de propriété `className` spécifiée lors de l'utilisation du composant Button, la valeur sera une chaine de caractères au lieu d'`undefined`.

### Exercices :

* lire plus à propos [des paramètres par défaut d'ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Default_parameters)

## Déclarations de composant

Maintenant vous avez quatre composants de classe ES6. Mais vous pouvez faire mieux. Laissez-moi vous présenter les composants fonctionnels stateless comme alternative de vos composants de classe ES6. Avant que vous refactorez vos composants, introduisons les différents types de composants React.

* **Functional Stateless Components:** Ces composants sont des fonctions qui ont une entrée et retourne une sortie. Les entrées sont les propriétés. La sortie est une instance de composant en JSX pur. Pour l'instant c'est assez similaire à une classe de composant ES6. Cependant, les composants fonctionnels stateless sont des fonctions (functional) et ils n'ont pas d'état local (stateless). Vous ne pouvez accéder ou mettre à jour l'état avec `this.state` ou `this.setState()` car il n'y a pas d'objet `this`. De plus, ils ne possèdent pas les méthodes de cycle de vie. Vous n'avez pas encore appris les méthodes de cycle de vie, mais vous en avez déjà utilisé deux : `constructor()` et `render()`. Tandis que le constructeur se lance une seule fois dans le temps de vie d'un composant, la méthode de classe `render()` se lance une première fois à l'initialisation et à chaque fois que le composant se met à jour. Garder en mémoire que les composants fonctionnels stateless n'ont pas de méthodes de cycle de vie, pour lorsque vous arriverez plus tard au chapitre sur les méthodes du cycle de vie.

* **ES6 Class Components:** Vous avez déjà utilisé ce type de déclaration de composant dans vos quatre composants. Dans la définition de classe, ils étendent le composant React. l'`extend` fournit tous les méthodes de cycle de vie, disponible au sein de l'API de composant React. De cette façon, vous serez en mesure d'utiliser la méthode de classe `render()`. De plus, vous pouvez stocker et manipuler l'état à l'intérieur des composants de classe ES6 en utilisant `this.state` et `this.setState()`.

* **React.createClass:** La déclaration de composant été utilisée dans les anciennes versions de React et encore dans les applications React ES5. Mais [Facebook la déclaré comme dépréciée](https://facebook.github.io/react/blog/2015/03/10/react-v0.13.html) en faveur du JavaScript ES6. Ils ont même ajouté un [warning de dépréciation dans la version 15.5](https://facebook.github.io/react/blog/2017/04/07/react-v15.5.0.html). Vous ne les l'utiliserez pas dans le livre.

Donc globalement il reste plus que deux déclarations de composant. Mais quand utiliser les composants fonctionnels stateless au lieu des composants de classe ES6? Une règle générale est d'utiliser les composants fonctionnels stateless lorsque vous n'avez pas besoin de l'état local ou des méthodes de cycle de vie du composant.
Habituellement vous débutez par implémenter vos composants en tant que composants fonctionnels stateless. Une fois que vous avez besoin d'accéder à l'état ou aux méthodes de cycle de vie, vous êtes obligé de le refactorer en un composant de classe ES6. Dans notre application, nous avons commencé dans le sens inverse pour les besoins de l'apprentissage de React.

Retournons à notre application. L'app component utilise l'état interne. C'est pourquoi il doit rester en tant que composant de classe ES6. Mais les trois autres composants de classes ES6 sont stateless. Ils n'ont pas besoin d'accéder à `this.state` ou `this.setState()`. Encore plus important, ils n'ont pas de méthodes de cycle de vie. Refactorons ensemble le composant Search en un composant fonctionnel stateless. Le refactor du composant Table et Button restera comme étant votre exercice.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function Search(props) {
  const { value, onChange, children } = props;
  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
# leanpub-end-insert
~~~~~~~~

C'est globalement tout. Les propriétés sont accessibles dans la signature de fonction et la valeur de retour est du JSX. Mais vous pouvez faire du code plus judicieux dans un composant fonctionnel stateless. Vous connaissez déjà la décomposition ES6. La meilleure pratique est de l'utiliser dans une signature de fonction pour décomposer les propriétés.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
function Search({ value, onChange, children }) {
# leanpub-end-insert
  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

Mais on peut faire mieux. Vous connaissez déjà les fonctions fléchées ES6 permettant de conserver vos fonctions concises. Vous pouvez supprimer le *block body* de la fonction. Dans un corps concis un retour implicit est attaché ainsi vous pouvez supprimer la déclaration du *return*. Puisque votre composant fonctionnel stateless est une fonction, vous pouvez également la conserver concise.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Search = ({ value, onChange, children }) =>
  <form>
    {children} <input
      type="text"
      value={value}
      onChange={onChange}
    />
  </form>
# leanpub-end-insert
~~~~~~~~

La dernière étape était spécifiquement utile pour garantir d'avoir seulement les propriétés en entrée et le JSX comme sortie. Rien d'autre entre. Pourtant, vous pouvez *do something* entre en utilisant un bloc dans votre fonction fléchée ES6.

{title="Code Playground",lang=javascript}
~~~~~~~~
const Search = ({ value, onChange, children }) => {

  // do something

  return (
    <form>
      {children} <input
        type="text"
        value={value}
        onChange={onChange}
      />
    </form>
  );
}
~~~~~~~~

Mais pour maintenant vous n'en aurez pas besoin. C'est pourquoi vous pouvez conserver la précédente version sans le *block body*. Lors de l'utilisation des *block bodies*, les personnes ont souvent tendance de faire trop de choses dans la fonction. En excluant le *block body*, vous pouvez vous concentrer sur l'entrée et la sortie de votre fonction.

Maintenant vous avez un composant fonctionnel stateless allégé. Une fois que vous aurez besoin d'accéder à son état interne de composant ou à ses méthodes de cycle de vie, vous le refactorerez en un composant de classe ES6. En plus vous voyez comment le JavaScript ES6 peut être utilisé dans les composants React pour les rendre plus concis et élégant.

### Exercices :

* refactorer le composant Table et Button en composants fonctionnels stateless
* lire plus à propos [ des composants de classe ES6 et des composants fonctionnels stateless](https://facebook.github.io/react/docs/components-and-props.html)

## Styliser les composants

Ajoutons quelques basiques styles pour votre application et vos composants. Vous pouvez réutiliser les fichiers *src/App.css* et *src/index.css*. Ces fichiers doivent toujours être dans votre projet puisque que vous l'avez initié avec *create-react-app*. Ils devraient aussi être importés dans vos fichiers *src/App.js* et *src/index.js*. J'ai préparé quelques CSS que vous pouvez simplement copier et coller vers ces fichiers, mais n'hésitez pas à utiliser votre propre style.

Tout d'abord, le style pour l'ensemble de votre application.

{title="src/index.css",lang="css"}
~~~~~~~~
body {
  color: #222;
  background: #f4f4f4;
  font: 400 14px CoreSans, Arial,sans-serif;
}

a {
  color: #222;
}

a:hover {
  text-decoration: underline;
}

ul, li {
  list-style: none;
  padding: 0;
  margin: 0;
}

input {
  padding: 10px;
  border-radius: 5px;
  outline: none;
  margin-right: 10px;
  border: 1px solid #dddddd;
}

button {
  padding: 10px;
  border-radius: 5px;
  border: 1px solid #dddddd;
  background: transparent;
  color: #808080;
  cursor: pointer;
}

button:hover {
  color: #222;
}

*:focus {
  outline: none;
}
~~~~~~~~

Deuxièmement, le style pour vos composants dans le fichier App.

{title="src/App.css",lang="css"}
~~~~~~~~
.page {
  margin: 20px;
}

.interactions {
  text-align: center;
}

.table {
  margin: 20px 0;
}

.table-header {
  display: flex;
  line-height: 24px;
  font-size: 16px;
  padding: 0 10px;
  justify-content: space-between;
}

.table-empty {
  margin: 200px;
  text-align: center;
  font-size: 16px;
}

.table-row {
  display: flex;
  line-height: 24px;
  white-space: nowrap;
  margin: 10px 0;
  padding: 10px;
  background: #ffffff;
  border: 1px solid #e3e3e3;
}

.table-header > span {
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0 5px;
}

.table-row > span {
  overflow: hidden;
  text-overflow: ellipsis;
  padding: 0 5px;
}

.button-inline {
  border-width: 0;
  background: transparent;
  color: inherit;
  text-align: inherit;
  -webkit-font-smoothing: inherit;
  padding: 0;
  font-size: inherit;
  cursor: pointer;
}

.button-active {
  border-radius: 0;
  border-bottom: 1px solid #38BB6C;
}
~~~~~~~~

Maintenant vous pouvez utiliser le style dans certains de vos composants. N'oubliez pas d'utiliser `className` de React plutôt que l'attribut HTML `class`.

Premièrement, appliquez-le dans votre composant de classe ES6 App.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {

  ...

  render() {
    const { searchTerm, list } = this.state;
    return (
# leanpub-start-insert
      <div className="page">
        <div className="interactions">
# leanpub-end-insert
          <Search
            value={searchTerm}
            onChange={this.onSearchChange}
          >
            Search
          </Search>
# leanpub-start-insert
        </div>
# leanpub-end-insert
        <Table
          list={list}
          pattern={searchTerm}
          onDismiss={this.onDismiss}
        />
# leanpub-start-insert
      </div>
# leanpub-end-insert
    );
  }
}
~~~~~~~~

Deuxièmement, appliquez-le dans votre composant fonctionnel stateless Table.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({ list, pattern, onDismiss }) =>
# leanpub-start-insert
  <div className="table">
# leanpub-end-insert
    {list.filter(isSearched(pattern)).map(item =>
# leanpub-start-insert
      <div key={item.objectID} className="table-row">
# leanpub-end-insert
        <span>
          <a href={item.url}>{item.title}</a>
        </span>
        <span>{item.author}</span>
        <span>{item.num_comments}</span>
        <span>{item.points}</span>
        <span>
          <Button
            onClick={() => onDismiss(item.objectID)}
# leanpub-start-insert
            className="button-inline"
# leanpub-end-insert
          >
            Dismiss
          </Button>
        </span>
# leanpub-start-insert
      </div>
# leanpub-end-insert
    )}
# leanpub-start-insert
  </div>
# leanpub-end-insert
~~~~~~~~

Maintenant que vous avez stylisé votre application et vos composants avec du CSS basique. Il devrait avoir un rendu décent. Comme vous savez, JSX mélange l'HTML et le JavaScript. Maintenant une personne peut défendre d'également ajouter du CSS dans tout çà. C'est appelé de l'*inline style*. Vous pouvez définir des objets JavaScript et les transmettre à l'attribut style d'un élément.

Gardons la largeur de colonne de Table adaptable en utilisant l'*inline style*.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Table = ({ list, pattern, onDismiss }) =>
  <div className="table">
    {list.filter(isSearched(pattern)).map(item =>
      <div key={item.objectID} className="table-row">
# leanpub-start-insert
        <span style={{ width: '40%' }}>
          <a href={item.url}>{item.title}</a>
        </span>
        <span style={{ width: '30%' }}>
          {item.author}
        </span>
        <span style={{ width: '10%' }}>
          {item.num_comments}
        </span>
        <span style={{ width: '10%' }}>
          {item.points}
        </span>
        <span style={{ width: '10%' }}>
          <Button
            onClick={() => onDismiss(item.objectID)}
            className="button-inline"
          >
            Dismiss
          </Button>
        </span>
# leanpub-end-insert
      </div>
    )}
  </div>
~~~~~~~~

Le style est en ligne maintenant. Vous pouvez définir les objets de style à l'extérieur de votre élément pour rendre le tout plus propre.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const largeColumn = {
  width: '40%',
};

const midColumn = {
  width: '30%',
};

const smallColumn = {
  width: '10%',
};
~~~~~~~~

Après quoi vous les utiliserez dans vos colonnes `<span style={smallColumn}>`.

En général, vous trouverez des opinions et des solutions différentes pour le style au sein de React. Pour le moment, vous utilisez du pur CSS et de l'*inline style*. Cela est suffisant pour débuter.

Je ne veux pas vous orienter ici, mais je veux vous donner un peu plus de posibilités d'options. Vous pouvez les lire et les appliquer de votre propre chef. Mais si vous êtes nouveau à React, Je vous recommande pour le moment de rester sur du CSS pur et en de l'*inline style*.

* [styled-components](https://github.com/styled-components/styled-components)
* [CSS Modules](https://github.com/css-modules/css-modules)

{pagebreak}

Vous avez appris les bases pour écrire votre propre application React! Récapitulons les derniers chapitres :

* React
  * utiliser `this.state` et `setState()` pour gérer votre état interne dun composant
  * passer des fonctions ou des méthodes de classe à votre element handler
  * utiliser les formulaires et évènements dans React pour ajouter des interactions
  * le flux de données unidirectionnel est un concept important dans React
  * favoriser les composants contrôlés (ou controlled components)
  * composer les composants avec children et des composants réutilisables
  * utilisation et implémentation des composants de classe ES6 et des composants fonctionnels stateless
  * les approches pour styliser vos composants
* ES6
  * les fonctions qui sont liées à une classe sont des méthodes de classe
  * décomposition d'objets et de tableaux
  * paramètres par défaut.
* Générale
  * les fonctions d'ordre supérieur

Encore une fois il fait sens de prendre une pause. Intérioriser les acquis et appliquer les de votre propre initiative. Vous pouvez expérimenter avec le code source que vous avez écrit jusqu'à présent. De plus, vous pouvez en découvrir davantage dans la [documentation officielle](https://facebook.github.io/react/docs/installation.html).

Vous pouvez trouver le code source dans le [dépôt officiel](https://github.com/rwieruch/hackernews-client/tree/4.2).
