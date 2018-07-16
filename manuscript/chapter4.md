# Organisation du code et tests

Ce chapitre se concentrera sur des sujets importants pour conserver un code maintenable dans une application scalable. Vous apprendrez à organiser le code adopter les meilleurs pratiques lors de la structuration de vos dossiers et fichiers. Un autre aspect vous apprendrez à tester, ce qui est primordial pour conserver un code robuste. Enfin, vous allez apprendre à utiliser un outil très utile pour le deboggage de vos applications React. La majeure partie du chapitre prendra du recul sur l'aspect pratique de l'application et expliquera quelques-uns de ces sujets pour vous.

## ES6 Modules : Import et Export

En JavaScript ES6 vous pouvez importer et exporter les fonctionnalités de modules. Ces fonctionnalités peuvent être des fonctions, classes, composants, constantes et autres. Globalement tous ce que vous pouvez assigner à une variable. Les modules peuvent être de simples fichiers ou des dossiers complets avec un fichier d'index en tant que point d'entrée.

Au début du livre, après que vous ayez démarré votre application avec *create-react-app*, vous avez déjà plusieurs déclarations d'`import` et d'`export` au sein de vos fichiers initiaux. Maintenant il est temps de les expliquer.

Les déclarations d'`import` et d'`export` vous aident à partager le code au travers de multiples fichiers. Avant il y avait déjà plusieurs solutions pour çà au sein de l'environnement JavaScript. C'était un bazar, car vous souhaitez suivre une façon standardisée plutôt que d'avoir plusieurs approches pour la même chose. Maintenant c'est un comportement natif depuis JavaScript ES6.

De plus ces déclarations adhèrent à la séparation de code. Vous distribuez votre code au travers de multiples fichiers pour le garder réutilisable et maintenable. Réutilisable car vous pouvez importer un morceau de code dans plusieurs fichiers. Maintenable car vous avez qu'une seule source où vous maintenez le morceau de code.

Enfin mais pas des moindres, il vous aide à penser en matière d'encapsulation de code. Pas toutes les fonctionnalités ont la nécessité d'être exportées depuis un fichier. Certaines de ces fonctionnalités devrait seulement être utilisé dans le fichier où ils ont été définies. Les exports d'un fichier sont simplement l'API publique du fichier. Seules les fonctionnalités exportées sont disponibles pour être réutilisé ailleurs. Cela suit la meilleure pratique de l'encapsulation.

Mais mettons-nous à la pratique. Comment ces déclarations d'`import` et d'`export` fonctionnent-ils? Les exemples suivants présentent les déclarations en partageant une ou plusieurs variables au travers de deux fichiers. À la fin, l'approche peut être mise à l'échelle pour de multiples fichiers et peut partager plus que de simples variables.

Vous pouvez exporter une ou plusieurs variables. C'est appelé un export nommé (named export).

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const firstname = 'robin';
const lastname = 'wieruch';

export { firstname, lastname };
~~~~~~~~

Et les importer dans un autre fichier avec un chemin relatif au premier fichier

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import { firstname, lastname } from './file1.js';

console.log(firstname);
// output: robin
~~~~~~~~

Vous pouvez aussi importer toutes les variables exporter depuis un autre fichier comme étant un seul objet.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import * as person from './file1.js';

console.log(person.firstname);
// output: robin
~~~~~~~~

Les imports peuvent avoir un alias. Cela peut arriver que vous importez des fonctionnalités de divers fichiers et qu'ils ont le même nom d'export. C'est pour cela que vous pouvez utiliser un alias.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import { firstname as foo } from './file1.js';

console.log(foo);
// output: robin
~~~~~~~~

Dernier point, mais pas des moindres il existe la déclaration `default`. Elle peut être utilisée pour plusieurs cas d'utilisation :

* exporter et importer une seule fonctionnalité
* mettre en exergue la fonctionnalité principale de l'API exporté d'un module
* avoir une fonctionnalité d'import de secours

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const robin = {
  firstname: 'robin',
  lastname: 'wieruch',
};

export default robin;
~~~~~~~~

Vous pouvez omettre les accolades pour l'import afin d'importer le default export.

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import developer from './file1.js';

console.log(developer);
// output: { firstname: 'robin', lastname: 'wieruch' }
~~~~~~~~

De plus, le nom d'import diffère du nom par défaut exporté. Vous pouvez aussi l'utiliser conjointement avec les exports nommés et les déclarations d'imports.

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
const firstname = 'robin';
const lastname = 'wieruch';

const person = {
  firstname,
  lastname,
};

export {
  firstname,
  lastname,
};

export default person;
~~~~~~~~

{title="Code Playground: file2.js",lang="javascript"}
~~~~~~~~
import developer, { firstname, lastname } from './file1.js';

console.log(developer);
// output: { firstname: 'robin', lastname: 'wieruch' }
console.log(firstname, lastname);
// output: robin wieruch
~~~~~~~~

Dans les exports nommés vous pouvez économiser des lignes additionnelles et exporter directement les variables.

{title="Code Playground: file1.js",lang="javascript"}
~~~~~~~~
export const firstname = 'robin';
export const lastname = 'wieruch';
~~~~~~~~

Ce sont les principales fonctionnalités des modules ES6. Ils vous aide à organiser votre code, maintenir votre code et concevoir des APIs de module réutilisable. Vous pouvez aussi exporter et importer des fonctionnalités pour les tester. Vous ferez cela dans l'un des chapitres suivants.

### Exercices :

* lire plus à propos [ES6 import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)
* lire plus à propos [ES6 export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export)

## Organisation du code avec les modules ES6

Vous pourriez vous demander : pourquoi nous n'avons pas suivi les meilleurs pratiques pour séparer notre code pour le fichier *src/App.js*? Dans le fichier nous avons déjà plusieurs composants qui pourrait être défini dans leurs propres (modules) fichiers/dossiers. Pour l'intérêt d'apprentissage de React, c'est pratique de conserver ces choses à un seul endroit. Mais une fois que votre application React grossit, vous devez considérer le fait de séparer ces composants à l'intérieur de plusieurs modules. La seule façon pour vos applications d'être mis à l'échelle.

Par conséquent, je proposerai plusieurs structures de module que vous "pourriez" appliquer. Je vous recommande de les appliquer en tant qu'exercice à la fin du livre. Pour conserver le livre simple, j'effectuerai pas la séparation de code et continuerai les chapitres suivants avec le fichier *src/App.js*.

Une structure de module possible serait :

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  index.css
  App.js
  App.test.js
  App.css
  Button.js
  Button.test.js
  Button.css
  Table.js
  Table.test.js
  Table.css
  Search.js
  Search.test.js
  Search.css
~~~~~~~~

Cela sépare les composants à l'intérieur de leurs propres fichiers, mais cela ne semble pas très prometteur. Vous pouvez voir beaucoup de duplication de nom et seules les extensions de fichier diffèrent. Une autre structure de module serait :

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  index.css
  App/
    index.js
    test.js
    index.css
  Button/
    index.js
    test.js
    index.css
  Table/
    index.js
    test.js
    index.css
  Search/
    index.js
    test.js
    index.css
~~~~~~~~

Cela semble plus propre qu'avant. La dénomination *index* d'un fichier le décrit comme étant le point d'entrée du dossier. C'est juste une convention de nommage commune, mais vous pouvez utiliser votre propre nommage également. Dans cette structure de module, un composant est défini par sa déclaration de composant dans le fichier JavaScript, mais aussi par son style et ses tests.

Une autre étape pourrait être l'extraction des variables de constantes du composant App. Ces constantes étaient utilisées pour composer l'URL de l'API d'Hacker News.

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  index.css
# leanpub-start-insert
  constants/
    index.js
  components/
# leanpub-end-insert
    App/
      index.js
      test.js
      index.css
    Button/
      index.js
      test.js
      index.css
    // ...
~~~~~~~~

Naturellement les modules devront être séparés à l'intérieur de *src/constants/* et *src/components/*. Maintenant le fichier *src/constants/index.js* pourrait ressembler à cela :

{title="Code Playground: src/constants/index.js",lang="javascript"}
~~~~~~~~
export const DEFAULT_QUERY = 'redux';
export const DEFAULT_HPP = '100';
export const PATH_BASE = 'https://hn.algolia.com/api/v1';
export const PATH_SEARCH = '/search';
export const PARAM_SEARCH = 'query=';
export const PARAM_PAGE = 'page=';
export const PARAM_HPP = 'hitsPerPage=';
~~~~~~~~

Le fichier *App/index.js* peut importer ces variables pour les utiliser.

{title="Code Playground: src/components/App/index.js",lang=javascript}
~~~~~~~~
import {
  DEFAULT_QUERY,
  DEFAULT_HPP,
  PATH_BASE,
  PATH_SEARCH,
  PARAM_SEARCH,
  PARAM_PAGE,
  PARAM_HPP,
} from '../../constants/index.js';

// ...
~~~~~~~~

Lorsque vous utilisez la convention de nommage *index.js*, vous pouvez omettre le nom du fichier du chemin relatif.

{title="Code Playground: src/components/App/index.js",lang=javascript}
~~~~~~~~
import {
  DEFAULT_QUERY,
  DEFAULT_HPP,
  PATH_BASE,
  PATH_SEARCH,
  PARAM_SEARCH,
  PARAM_PAGE,
  PARAM_HPP,
# leanpub-start-insert
} from '../../constants';
# leanpub-end-insert

// ...
~~~~~~~~

Mais qu'est-ce qui est derrière le nommage de fichier *index.js*? La convention a été introduite dans le monde de node.js. Le fichier index est un point d'entrée auprès du module. Cela décrit l'API publique envers le module. Considérez la stucture de module suivante construite pour la démonstration :

{title="Folder Structure",lang="text"}
~~~~~~~~
src/
  index.js
  App/
    index.js
  Buttons/
    index.js
    SubmitButton.js
    SaveButton.js
    CancelButton.js
~~~~~~~~

Le dossier *Buttons/* a plusieurs composants boutons définis dans ses fichiers distincts. Chaque fichier peut `export default` un composant spécifique le rendant disponible pour *Buttons/index.js*. Le fichier *Buttons/index.js* importe tous les différentes représentation de bouton et les exporte en tant qu'API de module publique.

{title="Code Playground: src/Buttons/index.js",lang="javascript"}
~~~~~~~~
import SubmitButton from './SubmitButton';
import SaveButton from './SaveButton';
import CancelButton from './CancelButton';

export {
  SubmitButton,
  SaveButton,
  CancelButton,
};
~~~~~~~~

Maintenant *src/App/index.js* peut importer les boutons à partir de l'API de module public localisé dans le fichier *index.js*.

{title="Code Playground: src/App/index.js",lang=javascript}
~~~~~~~~
import {
  SubmitButton,
  SaveButton,
  CancelButton
} from '../Buttons';
~~~~~~~~

En suivant cette contrainte, il serait malvenu d'atteindre d'autres fichiers que l'*index.js* dans le module. Cela casserait les règles d'encapsulation.

{title="Code Playground: src/App/index.js",lang=javascript}
~~~~~~~~
// Mauvaise pratique, ne faites pas çà
import SubmitButton from '../Buttons/SubmitButton';
~~~~~~~~

Maintenant vous savez comment vous pouvez refactorer votre code source dans des modules avec les contraintes d'encapsulation. Comme je l'ai dit, dans l'intérêt de garder le livre simple je n'appliquerai pas ces changements. mais vous devrez faire le refactoring lorsque vous avez terminé la lecture du livre.

### Exercices :

* refactorer votre fichier *src/App.js* en plusieurs modules de composant lorsque vous avez terminé le livre

## Tests instantanés avec Jest

Le livre ne plongera pas profondément à l'intérieur du sujet des tests, mais il ne doit pas être omis. Tester votre code en programmation est essentiel et doit être perçu comme obligatoire. Vous souhaitez conserver une bonne qualité de votre code et une assurance que tout fonctionne.

Peut-être avez-vous entendu la pyramide de tests. Il y a les tests end-to-end, les tests d'intégration et les tests unitaires. Si vous n'êtes pas familier avec cela, le livre donne un rapide et basique aperçu. Un test unitaire est utilisé pour tester un bloc de code isolé et petit. Cela peut être une fonction seule qui est testée par un test unitaire. Cependant, parfois les tests unitaires fonctionnent en l'isolant mais ne fonctionnent plus en les combinant avec d'autres tests unitaires. Ils ont besoin d'être testés en tant que groupe de tests unitaires. C'est là où les tests d'intégration peuvent aider en couvrant le fait que les tests unitaires fonctionnent ensemble. Enfin, mais pas des moindres, un test end-to-end est la simulation d'un scénario utilisateur réel. Cela peut être une installation automatisée au sein du navigateur simulant les étapes d'authentification d'un utilisateur au sein d'une application web. Tandis que les tests unitaires sont rapides et faciles à écrire et maintenir, les tests end-to-end sont le total opposé.

Combien de tests de chaque type ai-je besoin? Vous souhaitez avoir plusieurs tests unitaires pour couvrir vos fonctions isolées. Après quoi, vous pouvez avoir plusieurs tests d'intégration pour couvrir le fait que les fonctions les plus importantes fonctionnent de façons combinées comme attendues. Enfin, mais pas des moindres vous pourrez vouloir avoir seulement quelques tests end-to-end pour simuler les scénarios critiques de votre application web. C'est tout pour l'introdution générale au sein du monde des tests.

Donc comment appliquer ces connaissances de tests pour votre application React? La base pour les tests au sein de React est les tests de composant qui peut être vulgarisé en tant que tests unitaires et une certaine partie en tant que tests instantanés.  Vous mènerez les tests unitaires pour vos composants dans le prochain chapitre en utilisant une bibliothèque appelée Enzyme. Dans cette section du chapitre, vous vous concentrerez sur un autre type de tests : les tests d'instantanés. C'est là que Jest rentre en jeu.

[Jest](https://jestjs.io/) est un framework JavaScript de tests qui est utilisé par Facebook. Dans la communauté React, c'est utilisé pour les tests de composant React. Heureusement *create-react-app* possède déjà Jest, donc vous n'avez pas besoin de vous préoccuper de son installation.

Démarrons le test de vos premiers composants. Avant de pouvoir faire cela, vous devez exporter les composants, que vous allez tester, depuis votre fichier *src/App.js*. Après quoi vous pouvez les tester dans un fichier différent. Vous avez appris cela dans le chapitre portant sur l'organisation du code.

{title="src/App.js",lang=javascript}
~~~~~~~~
// ...

class App extends Component {
  // ...
}

// ...

export default App;

# leanpub-start-insert
export {
  Button,
  Search,
  Table,
};
# leanpub-end-insert
~~~~~~~~

Dans votre fichier *App.test.js*, vous trouverez un premier test qui a été constuit avec create-react-app*. Il vérifie que le composant App fera un rendu sans erreur.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';

it('renders without crashing', () => {
  const div = document.createElement('div');
  ReactDOM.render(<App />, div);
  ReactDOM.unmountComponentAtNode(div);
});
~~~~~~~~

Le block "it" décrit un cas de test. Il arrive avec une description de test et lorsque vous le testez, il peut soit réussir soit échouer. De plus, vous pouvez l'englober à l'intérieur d'un bloc "describe" qui définit votre suite de tests. Une suite de tests peut inclure un certain nombre de blocs "it" pour un composant spécifique. Vous verrez ces blocs "describe" après. Les deux blocs sont utilisés pour séparer et organiser vos cas de tests.

Notez que la fonction `it` est connu dans la communauté JavaScript comme la fonction où vous lancez un test unique. Cependant, dans Jest c'est souvent perçu comme un alias de fonction `test`.

Vous pouvez lancer vos cas de tests en utilisant le script de test interactif de *create-react-app* en ligne de commande. Vous obtiendrez la sortie pour tous les cas de tests sur votre interface en ligne de commande.

{title="Command Line",lang="text"}
~~~~~~~~
npm test
~~~~~~~~

Maintenant Jest vous permet d'écrire des tests d'instantanés. Ces tests font un instantané de votre composant rendu et lancent cet instantané par rapport à de futurs instantanés. Lorsqu'un futurs intantané change, vous serez informé dans le test. Vous pouvez soit accepter le changement d'instantané, car vous avez changé l'implémentation du composant délibérément, ou rejeter le changement et investiguer sur l'erreur. Il complète les tests unitaires très bien, car vous testez seulement les différences sur la sortie rendue. Il n'ajoute pas un important coût de maintenance, car vous pouvez simplement accepter les instantanés modifiés lorsque vous changez quelques chose délibérément pour la sortie de rendu de votre composant.

Jest stocke les instantanés dans un dossier. C'est seulement de cette façon qu'il peut valider la différence par rapport à un instantané futur.

Avant l'écriture de votre premier test instantané avec Jest, vous devez installer une bibliothèque utilitaire.

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev react-test-renderer
~~~~~~~~

Maintenant vous pouvez étendre le test du composant App avec votre premier test instantané. Premièrement, importer la nouvelle fonctionnalité depuis le node package et englober votre précédent bloc "it" du composant App dans un bloc de description "describe". Dans ce cas, la suite de test est consacré au composant App.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
# leanpub-start-insert
import renderer from 'react-test-renderer';
# leanpub-end-insert
import App from './App';

# leanpub-start-insert
describe('App', () => {

# leanpub-end-insert
  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<App />, div);
    ReactDOM.unmountComponentAtNode(div);
  });

# leanpub-start-insert

});
# leanpub-end-insert
~~~~~~~~

Maintenant vous pouvez implémenter votre premier instantané en utilisant un bloc "test".

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
import App from './App';

describe('App', () => {

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<App />, div);
    ReactDOM.unmountComponentAtNode(div);
  });

# leanpub-start-insert
  test('has a valid snapshot', () => {
    const component = renderer.create(
      <App />
    );
    const tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });
# leanpub-end-insert

});
~~~~~~~~

Lancez de  nouveau vos tests et observez comment les tests réussissent ou échouent. Ils devraient réussir. Une fois que vous modifiez la sortie du bloc de rendu de votre composant App, le test instantané doit échouer. Alors vous pouvez décider de mettre à jour l'instantané ou investiguer dans votre composant App.

Globalement la fonction `renderer.create()` crée un instantané de votre composant App. Il le rend virtuellement et stocke le DOM à l'intérieur de l'instantané. Après quoi, l'instantané est attendu pour correspondre avec le précédent instantané au moment où vous avez lancé vos tests d'instantané. De cette façon, vous pouvez assurer que votre DOM reste le même et que rien ne change par accident.

Ajoutons plus de tests pour nos composants indépendants. Premièrement, le composant Search :

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
# leanpub-start-insert
import App, { Search } from './App';
# leanpub-end-insert

// ...

# leanpub-start-insert
describe('Search', () => {

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Search>Search</Search>, div);
    ReactDOM.unmountComponentAtNode(div);
  });

  test('has a valid snapshot', () => {
    const component = renderer.create(
      <Search>Search</Search>
    );
    const tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});

# leanpub-end-insert
~~~~~~~~

Le composant Search a deux tests similaires au composant App. Le premier test rend simplement le composant Search auprès du DOM et vérifie qu'il n'y est pas d'erreur durant le processus de rendu. S'il y aurait une erreur, le test casserait même s'il n'y a pas d'assertion. (ex : expect, match, equal) dans le bloc de test. Le second test d'instantané est utilisé pour stocker un instantané du composant rendu et de le lancer par rapport au précédent instantané. Il échoue lorsque l'instantané a changé.

Deuxièmement, vous pouvez tester le composant Button auquel les mêmes principes de tests que le composant Search sont appliqués.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
// ...
# leanpub-start-insert
import App, { Search, Button } from './App';
# leanpub-end-insert

// ...

# leanpub-start-insert
describe('Button', () => {

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Button>Give Me More</Button>, div);
    ReactDOM.unmountComponentAtNode(div);
  });

  test('has a valid snapshot', () => {
    const component = renderer.create(
      <Button>Give Me More</Button>
    );
    const tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
# leanpub-end-insert
~~~~~~~~

Enfin, mais pas des moindres, le composant Table auquel vous pouvez passer un ensemble de `props` initiales dans le but de le rendre avec un échantillon de liste.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
// ...
# leanpub-start-insert
import App, { Search, Button, Table } from './App';
# leanpub-end-insert

// ...

# leanpub-start-insert
describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
  };

  it('renders without crashing', () => {
    const div = document.createElement('div');
    ReactDOM.render(<Table { ...props } />, div);
  });

  test('has a valid snapshot', () => {
    const component = renderer.create(
      <Table { ...props } />
    );
    const tree = component.toJSON();
    expect(tree).toMatchSnapshot();
  });

});
# leanpub-end-insert
~~~~~~~~

Les tests d'instantané habituellement restent assez basiques. Vous voulez seulement couvrir le fait que le composant ne change pas sa sortie. Une fois qu'il change la sortie, vous devez décider si vous acceptez les changements. Autrement vous devez réparer le composant lorsque la sortie n'est pas une sortie désirée.

### Exercices :

* regarder comment un test d'instantané échoue une fois que vous changez votre valeur de retour du composant avec la méthode `render()`
  * acceptez ou rejetez le changement d'instantané
* conservez vos instantanés à jour lorsque l'implémentation de composant change dans les prochains chapitres
* lire plus à propos de [Jest au sein de React](https://jestjs.io/docs/en/tutorial-react)

## Les tests unitaires avec Enzyme

[Enzyme](https://github.com/airbnb/enzyme) est un utilitaire de test d'Airbnb pour assert, manipuler et traverser vos composants React. Vous pouvez l'utiliser pour piloter vos tests unitaires dans le but de compléter vos tests d'instantanés dans React.

Regardons comment vous pouvez utiliser Enzyme. Premièrement vous devez l'installer puisqu'il n'est pas embarqué dans *create-react-app*. Il arrive avec une extension pour l'utiliser au sein de React.

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev enzyme react-addons-test-utils enzyme-adapter-react-16
~~~~~~~~

Deuxièmement, vous aurez besoin de l'inclure dans votre installation de test et initialiser son Adapter pour son usage au sein de React.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
# leanpub-start-insert
import Enzyme from 'enzyme';
import Adapter from 'enzyme-adapter-react-16';
# leanpub-end-insert
import App, { Search, Button, Table } from './App';

# leanpub-start-insert
Enzyme.configure({ adapter: new Adapter() });
# leanpub-end-insert
~~~~~~~~

Maintenant, vous pouvez écrire votre premier test unitaire dans le bloc "description" de Table. Vous utiliserez `shallow()` pour rendre votre composant et affirmer que la Table possède deux objets, car vous lui passez deux éléments de liste. L'affirmation vérifie simplement si l'élément possède deux éléments avec la classe `table-row`.

{title="src/App.test.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import renderer from 'react-test-renderer';
# leanpub-start-insert
import Enzyme, { shallow } from 'enzyme';
# leanpub-end-insert
import Adapter from 'enzyme-adapter-react-16';
import App, { Search, Button, Table } from './App';

// ...

describe('Table', () => {

  const props = {
    list: [
      { title: '1', author: '1', num_comments: 1, points: 2, objectID: 'y' },
      { title: '2', author: '2', num_comments: 1, points: 2, objectID: 'z' },
    ],
  };

  // ...

# leanpub-start-insert
  it('shows two items in list', () => {
    const element = shallow(
      <Table { ...props } />
    );

    expect(element.find('.table-row').length).toBe(2);
  });
# leanpub-end-insert

});
~~~~~~~~

*Shallow* rend le composant sans ses composants enfants. De cette façon, vous pouvez faire le test dévoué à un seul composant.

Enzyme a dans l'ensemble trois mécanismes de rendu dans son API? Vous connaissez déjà `shallow()`, mais il y existe aussi `mount()` and `render()`. Les deux instancient les instances du composant parent ainsi que tous les composants enfants. De plus `mount()` vous donne un accès aux méthodes de cycle de vie du composant. Mais quand savoir utiliser quel mécanisme de rendu? Ici quelques règles générales :

* Toujours débuter avec un test shallow
* Si `componentDidMount()` ou `componentDidUpdate()` doit être testé, utiliser `mount()`
* Si vous voulez tester le cycle de vie du composant et le comportement des enfants, utiliser `mount()`
* Si vous voulez tester un rendu des enfants du composant avec moins de contrainte que `mount()` et que vous n'êtes pas intéressé par les méthodes du cycle de vie, utilisez `render()`

Vous pouvez continuer à tester unitairement vos composants. Mais assurez-vous de conserver les tests simple et maintenable. Autrement vous devrez les refactorer une fois que vous modifiez vos composants. C'est pourquoi Facebook a introduit les tests instantanés avec Jest en premier lieu.

### Exercices :

* écrire un test unitaire avec Enzyme pour votre composant Button
* conserver vos tests unitaires à jour durant le chapitre suivant
* lire plus à propos d'[Enzyme et de son API de rendu](https://github.com/airbnb/enzyme)
* lire plus à propos du [testing des applications React](https://www.robinwieruch.de/react-testing-tutorial)

## Interface de composant avec les PropTypes

Vous pourriez connaitre [TypeScript](https://www.typescriptlang.org/) ou [Flow](https://flowtype.org/)  pour introduire une interface de type pour JavaScript. Un langage typé est plus robuste aux errors, car le code sera validé en fonction de son propre code. Les éditeurs et autres utilitaires peuvent saisir ces erreurs avant que le programme soit lancé. Cela rend votre programme plus robuste.

Dans le livre, vous ne serez pas introduit à Flow ou TypeScript, mais à une autre façon proche de tester vos types dans les composants. React arrive avec un vérificateur de type embarqué pour empêcher les bugs. Vous pouvez utiliser *PropTypes* pour décrire votre interface de composant. Toutes les propriétés qui sont passées depuis un composant parent à un composant enfant seront validées selon l'interface *PropTypes* assignée au composant enfant.

Cette section du chapitre vous montera comment vous pouvez rendre tous vos types de composants sûrs avec *PropTypes*. J'omettrai ces changements pour les prochains chapitres, car ils ajoutent du remaniement de code inutile. Mais vous pouvez les conserver et les mettre à jour tout du long pour conserver votre type d'interface de composants sûrs.

Premièrement, vous devez installer un package séparé pour React.

{title="Command Line",lang="text"}
~~~~~~~~
npm install prop-types
~~~~~~~~

Maintenant, vous pouvez importer *les PropTypes*.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import axios from 'axios';
# leanpub-start-insert
import PropTypes from 'prop-types';
# leanpub-end-insert
~~~~~~~~

Débutons en assignant une interface de propriétés aux composants :

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

# leanpub-start-insert
Button.propTypes = {
  onClick: PropTypes.func,
  className: PropTypes.string,
  children: PropTypes.node,
};
# leanpub-end-insert
~~~~~~~~

Tout simplement. Vous prenez chaque argument depuis la signature de fonction et lui assigner un *PropType*. Le *PropTypes* basiques pour les primitives et les objets complexes sont :

* PropTypes.array
* PropTypes.bool
* PropTypes.func
* PropTypes.number
* PropTypes.object
* PropTypes.string

De plus vous avez deux PropTypes supplémentaires pour définir un fragment (node) présentable, ex : une string, et un élément React :

* PropTypes.node
* PropTypes.element

Vous utilisez déjà le *PropType* `node` pour le composant Button. Globalement il y a des définitions de *PropType* supplémentaires que vous pouvez étudier dans la documentation officielle de React.

Pour le moment tout les *PropTypes* définies pour le Button sont optionnelles. Les paramètres peuvent être `null` ou `undefined`. Mais pour certaines propriétés vous voudriez imposer le fait qu'elles soient définies. Vous pouvez rendre nécessaire le fait que ces propriétés sont passées au composant.

{title="src/App.js",lang=javascript}
~~~~~~~~
Button.propTypes = {
# leanpub-start-insert
  onClick: PropTypes.func.isRequired,
# leanpub-end-insert
  className: PropTypes.string,
# leanpub-start-insert
  children: PropTypes.node.isRequired,
# leanpub-end-insert
};
~~~~~~~~

La `className` n'est pas requise, car elle peut par défaut être une chaine de caractère vide. Après vous définirez une interface *PropTypes* pour le composant Table :

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
Table.propTypes = {
  list: PropTypes.array.isRequired,
  onDismiss: PropTypes.func.isRequired,
};
# leanpub-end-insert
~~~~~~~~

Vous pouvez définir le contenu d'un *PropType* tableau plus explicitement :

{title="src/App.js",lang=javascript}
~~~~~~~~
Table.propTypes = {
  list: PropTypes.arrayOf(
    PropTypes.shape({
      objectID: PropTypes.string.isRequired,
      author: PropTypes.string,
      url: PropTypes.string,
      num_comments: PropTypes.number,
      points: PropTypes.number,
    })
  ).isRequired,
  onDismiss: PropTypes.func.isRequired,
};
~~~~~~~~

Seul l'`objectID` est requis, car vous savez que certaines parties de votre code en dépendent. Les autres propriétés sont seulement affichées, ainsi elles ne sont pas nécessairement requises. De plus vous ne pouvez être sûr que l'API d'Hacker News a toujours une propriété définie pour chaque objet dans le tableau.

C'est tout pour les *PropTypes*. Mais il y a un autre aspect. Vous pouvez définir des propriétés par défaut dans votre composant. Prenons de nouveau le composant Button. La propriété `className` a un paramètre ES6 par défaut dans la signature du composant.

{title="src/App.js",lang=javascript}
~~~~~~~~
const Button = ({
  onClick,
  className = '',
  children
}) =>
  // ...
~~~~~~~~

Vous pouvez le remplacer avec la propriété par défaut interne de React :

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
const Button = ({
  onClick,
  className,
  children
}) =>
# leanpub-end-insert
  <button
    onClick={onClick}
    className={className}
    type="button"
  >
    {children}
  </button>

# leanpub-start-insert
Button.defaultProps = {
  className: '',
};
# leanpub-end-insert
~~~~~~~~

Identique que le paramètre par défaut d'ES6, la propriété par défaut assure que la propriété est établie à une valeur par défaut lorsque le composant parent n'est pas spécifié. La vérification du type de *PropType* se produit après que la propriété par défaut est évaluée.

Si vous lancez de nouveau vos tests, vous pourriez voir les erreurs *PropTypes* de vos composants dans votre terminal. Elles peuvent apparaitre car vous n'avez pas défini toutes les propriétés de vos composants dans les tests qui sont définis comme requises dans votre définition de *PropType*. Cependant les tests eux-même passent correctement. Vous pouvez passer toutes les propriétés requises aux composants dans vos tests pour éviter ces erreurs.

### Exercices :

* définir l'interface *PropType* pour le composant Search
* ajouter et mettre à jour les interfaces *PropType* lorsque vous ajoutez et mettez à jour des composants dans les prochains chapitres
* lire plus à propos des [PropTypes de React](https://reactjs.org/docs/typechecking-with-proptypes.html)

## Débuggage à l'aide du React Developer Tools

Cette dernière section vous présente un outil très utile, généralement utilisé pour inspecter et debugger les applications React. [React Developer Tools](https://github.com/facebook/react-devtools) vous laisse inspecter la hiérarchie des composants React, les props et l'état. C'est une extension de navigateur web (pour l'heure, sur Chrome et Firefox) et une application standalone (qui fonctionne sur d'autres environnements).

Une fois installé, l'icône de l'extension s'éclairera sur les sites web utilisant React. Sur ces sites web, vous verrez un onglet appelé "React" au sein de votre browser developer tools.

Essayons cela sur notre application Hacker News. Sur la plupart des navigateurs, un moyen rapide pour faire apparaître le _dev tools_ up consiste à réaliser un click droit sur la page et allez sur “Inspecter”. Faites-le lorsque votre application est chargée, puis cliquez sur l'onglet "React". Vous devriez voir sa hiérarchie d'éléments, avec `<App>` l'élément racine. Si vous le développez, vous trouverez également des instances de vos composants `<Search>`, `<Table>` et `<Button>`.

L'extension montre sur le panneau latéral l'état du composant ainsi que les props de l'élément sélectionné. Par exemple, si vous cliquez sur `<App>`, vous verrez qu'il ne dispose d'aucune props, mais qu'il a un état. C'est une technique de débuggage évidente pour monitorer l'état changeant de votre application dû à l'interaction de l'utilisateur.

Premièrement, vous vérifierez l'option "Highlight Updates" (habituellement au-dessus de l'arbre des éléments). Deuxièmement, vous pouvez taper un terme de recherche différent dans le champ d'entrée de l'application. Comme vous le remarquerez, seulement `searchTerm` sera modifié au sein de l'état du composant. Vous connaissez d'ores et déjà ce qui se produit, mais maintenant vous pouvez voir que cela fonctionne comme prévu.

Finalement, vous pouvez appuyer sur le bouton "Search". L'état `searchKey` sera immédiatement changé par la même valeur que `searchTerm` et quelques secondes après l'objet de réponse sera ajouté à `results`. la nature asynchrone de votre code est maintenant visible à l'oeil.

Enfin mais pas des moindres, si vous effectuer un click droit sur n'importe quel élément, un dropdown menu vous montrera plusieurs options utiles. Par exemple, vous pouvez copier les props ou le nom de l'élément, trouver le noeud du DOM correspondant ou sauter vers le code source de l'application au sein du navigateur web. Cette dernière option est très utile pour insérer des points d'arrêt dans le but de debugger les fonctions JavaScript.

### Exercises:

* Installer l'extension [React Developer Tools](https://github.com/facebook/react-devtools) sur votre navigateur web favoris
	* lancer votre application clone d'Hacker News et l'inspecter en utilisant l'extension
	* Experimenter les changements d'état et de props
	* Observer ce qui se produit lorsque vous déclencher une requête asynchrone
	* Performer plusieurs requêtes, en en répétant certaines. Observer que le mécanisme de cache fonctionne
+* lire à propos de [comment débugger vos fonctions JavaScript au sein d'un navigateur web](https://developers.google.com/web/tools/chrome-devtools/javascript/)

{pagebreak}

Vous avez appris comment organiser votre votre et comment le tester! Récapitulons les derniers chapitres :

* React
  * PropTypes vous laissent définir les vérifications de type pour les composants
  * Jest vous permet d'écrire des tests instantanés pour vos composants
  * Enzyme vous permet d'écrire des tests unitaires pour vos composants
  * React Developer Tools est un outil de débuggage très utile
* ES6
  * les déclarations d'import et d'export vous aident à organiser votre code
* Général
  * L'organisation de code vous permet de mettre à l'échelle votre application avec les meilleurs pratiques

Vous pouvez trouver le code source dans le [dépôt officiel](https://github.com/the-road-to-learn-react/hackernews-client/tree/5.4).
