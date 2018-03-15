# Introduction à React

Ce chapitre fournit une introduction à React. Peut-être vous demander vous: Pourquoi devrais-je apprendre React? L'objectif de ce chapitre est de répondre à cette interrogation. Après quoi, nous plongerons à l'intérieur de l'écosystème en débutant par votre première application React à partir de rien nécessitant zéro configuration. Durant cette phase, vous serez introduit à JSX ainsi qu'au ReactDOM. Soyez prêt pour vos premiers composants React.

## Salut, mon nom est React.

**Pourquoi devrez-vous, vous embêter à apprendre React**
Ces dernières années les applications web monopage ([SPA](https://en.wikipedia.org/wiki/Single-page_application)) sont devenu populaires. Les frameworks comme Angular, Ember et Backbone aident les développeurs JavaScript à construire des applications web modernes au-delà du simple usage de Vanilla JavaScript et JQuery. La liste de ces solutions populaires n'est pas exhaustive. Il existe un large panel de frameworks pour SPA. Quand on compare les dates de sorties officielles, la plupart d'entre eux font partie de la première génération de SPAs: Angular 2010, Backbone 2010, Ember 2011.

La sortie officielle de React par Facebook date de 2013. React n'est pas un framework SPA à proprement parler mais une bibliothèque gérant que la vue. C'est le "V" du fameux [MVC](https://en.wikipedia.org/wiki/Model–view–controller) (Model View Controller). Il permet seulement de rendre des composants en tant qu'élément visionnable dans le navigateur. Naturellement, l'écosystème autour de React rend possible la création de SPAs.

Mais pourquoi vous devrez utiliser React plutôt que la première génération de frameworks SPA? Tandis que la première génération de frameworks essayèrent de résoudre beaucoup de problématiques à la fois, React vous assiste seulement pour construire votre couche: vue. C'est une bibliothèque et non un framework. L'idée derrière cela : votre vue est une hiérarchie de composants composables.

Dans React vous pouvez conserver votre attention sur la couche : vue avant d'introduire plus de concepts pour votre application. Tous les autres concepts sont d'autres *building block* pour votre SPA. Ces *building blocks* sont essentiels pour construire une application mûre. Ils arrivent avec deux avantages.

Tout d'abord, vous pouvez apprendre les constructions de blocs étape par étape. Vous n'avez pas à vous inquiéter à propos de l'agencement de tout cela. Ceci est différent d'un framework qui vous fournit des blocs préconstruits dès le début. Ce livre se concentre sur React pour construire en tant que premier bloc de construction. Plus de blocs de construction suivront finalement.
Tout d'abord, vous pouvez apprendre les *building block* étape par étape. Vous n'avez pas à vous inquiéter à propos de l'agencement de tout cela. Ceci est différent d'un framework qui vous fournit des *building block* dès le début. Ce livre se concentre sur React en tant que premier *building block*. Plus de *building block* suivront finalement.

Dans un second temps, tout *building blocks* sont interchangeable. Cela rend l'écosystème autour de React innovant. Plusieurs solutions s'affrontent entre eux. Vous pouvez dès lors récupérer la solution la plus attrayante pour vous et votre cas d'utilisation.

La première génération de frameworks SPA arrivèrent à un déploiement en entreprise. Ils sont plus rigides. Tandis que React demeure innovant et tand à être adopté par plusieurs têtes pensantes d'entreprise leader tel que [Airbnb, Netflix et bien sûr Facebook](https://github.com/facebook/react/wiki/Sites-Using-React). Tous investissent dans le futur de React et satisfait et de react et de son écosystème.

De nos jours, React est probablement l'un des meilleurs choix pour construire das applications web modernes. Il délivre seulement la couche vue, [mais l'écosystème React fournit un framework entièrement souple et interchangeable](https://www.robinwieruch.de/essential-react-libraries-framework/). React dispose d'une API épuré, d'un incroyable écosystème et d'une grande communauté. Vous pouvez lire à propos de mes expériences [pourquoi je suis passé d'Angular à React](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/). Je recommande d'avoir une compréhension de pourquoi vous souhaitez choisir React plutôt qu'un autre framework ou bibliothèque. Après tout, tout le monde est enjoué à l'idée d'expérimenter vers où React nous conduira dans les prochaines années.

### Exercices

* lire plus à propos [pourquoi je suis passé d'Angular à React](https://www.robinwieruch.de/reasons-why-i-moved-from-angular-to-react/)
* lire plus à propos [l'écosystème souple de React](https://www.robinwieruch.de/essential-react-libraries-framework/)
* lire plus à propos de [comment apprende un framework](https://www.robinwieruch.de/how-to-learn-framework/)

## Prérequis

Si vous êtes issue d'un autre framework ou d'une bibliothèque pour SPA, vous devriez déjà être familier avec les bases du web développement. Si vous avez juste débuté en développement web, vous devriez vous sentir assez confortable avec HTML, CSS et JavaScript ES5 pour apprendre React. Ce livre fera une transition douce vers JavaScript ES6 et au-delà. Je vous encourage à rejoindre le  [Groupe Slack] officiel (https://slack-the-road-to-learn-react.wieruch.com/) du livre pour être aidé ou aider les autres.

### Éditeur et terminal

Qu'en est-il de l'environnement de développement? Vous aurez besoin d'un éditeur ou d'un IDE et d'un terminal (*command line tool*). Vous pouvez [suivre mon guide d'installation](https://www.robinwieruch.de/developer-setup/). C'est illustré pour les utilisateurs de MacOS, mais vous pouvez remplacer la plupart des outils pour un autre système d'exploitation. Il y a une pléthore d'articles qui vous montreront comment installer un environnement de développement pour le web et de façon plus élaborée pour votre OS.

Optionnellement, vous pouvez utiliser Git et Github par vous-même, pendant la réalisation des exercices du livre, pour conserver vos projets et la progression dans les dépôts de Github. Il existe un [petit guide](https://www.robinwieruch.de/git-essential-commands/) sur comment utiliser ces outils. Mais encore une fois, ce n'est pas obligatoire pour le livre et peut devenir contraignant lors de l'apprentissage de tout cela en même temps à partir de rien. Donc vous pouvez sauter cela si vous êtes un novice dans le développement web pour vous concentrer sur les parties essentielles apprises dans ce livre.

### Node et NPM

Dernier point, mais pas des moindres, vous aurez besoin d'une installation de [node and npm](https://nodejs.org/en/). Les deux sont utilisés pour gérer les bibliothèques dont vous aurez besoin tout du long. Dans ce livre, vous installerez des *external node packages* via npm (node package manager). Ces *node packages* peuvent être des bibliothèques ou des frameworks en entier.

Vous pouvez vérifier vos versions de node et npm en ligne de commande. Si vous n'obtenez aucune sortie sur le terminal, vous aurez au préalable besoin d'installer node et npm. Ce sont seulement mes versions au moment où j'écris ce livre :

{title="Command Line",lang="text"}
~~~~~~~~
node --version
*v8.9.4
npm --version
*v5.6.0
~~~~~~~~

## node et npm

Ce chapitre vous donne un petit aperçu de node et npm. Ceci n'est pas exhaustif, mais vous aurez tous les outils nécessaires. Si vous êtes familier avec les deux technologies, vous pouvez sauter ce chapitre.

Le **node package manager** (npm) vous permet d'installer des externes en ligne de commande. Ces packages peuvent être un ensemble de fonctions utilitaires, bibliothèques ou un framework complet. Ils sont les dépendances de votre application. Vous pouvez soit installer ces packages dans votre dossier node package globalement ou dans votre dossier de projet local.

Les nodes packages globaux sont accessibles depuis partout au sein du terminal et vous devez les installer seulement une seule fois dans votre répertoire. Vous pouvez installer un package global en tapant dans votre terminal :

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g <package>
~~~~~~~~

Le flag `-g` demande à npm d'installer le package globalement. Les packages locaux sont utilisés dans votre application. Par exemple, React en tant que bibliothèque sera un package local qui peut être requis pour l'utilisation de votre application. Vous pouvez l'installer via le terminal en tapant :

{title="Command Line",lang="text"}
~~~~~~~~
npm install <package>
~~~~~~~~

Dans le cas de React cela pourrait être :

{title="Command Line",lang="text"}
~~~~~~~~
npm install react
~~~~~~~~

Le package installé apparaîtra automatiquement  dans le dossier appelé *node_modules/* et sera listé dans le fichier *package.json* à côté de vos autres dépendances.

Mais comment initialiser pour la première fois le dossier *node_modules/* ainsi que le fichier *package.json* pour votre projet? Il y a une commande npm pour initialiser un projet npm et ainsi un fichier *package.json*. Seulement lorsque vous avez ce fichier, vous pouvez installer des nouveaux packages locaux via npm.

{title="Command Line",lang="text"}
~~~~~~~~
npm init -y
~~~~~~~~

Le flag `-y` est un raccourci pour initialisé tout par défaut dans votre *package.json*. Si vous n'utilisez pas le flag, vous devez décider de comment configurer le fichier. Après l'initialisation pour le projet npm vous êtes prêt à installer des nouveaux packages via `npm install <package>`.


Un dernier commentaire à propos du *package.json*. Le fichier vous permet de partager votre projet avec les autres développeurs sans partager tous les nodes packages. Le fichier possède toutes les références des nodes packages utilisés dans votre projet. Ces packages sont appelés dépendances. Tout le monde peut copier votre projet sans les dépendances. Les dépendances sont des références dans le *package.json*. Quelqu'un qui copie votre projet peut simplement installer tous les packages en utilisant `npm install` en ligne de commande.  Le script `npm install` prend toutes les dépendances listées dans le fichier *package.json* et les installent dans le dossier *node_modules/*.

Je veux traiter une commande npm supplémentaire : 

{title="Command Line",lang="text"}
~~~~~~~~
npm install --save-dev <package>
~~~~~~~~

Le flag `--save-dev` indique que le node package est seulement utilisé dans le développement d'environnement. Il ne sera pas utilisé en production que lorsque vous déployez votre application sur un serveur. Quel type de node package peut-être concerné? Imaginez-vous souhaiter tester votre application avec l'aide d'un node package. Vous aurez besoin d'installer le package via npm, mais vous voulez l'exclure de votre environnement de production. Les tests devront seulement s'exécuter durant la phase de développement et en aucun cas lorsque votre application est déjà lancée en production. À ce moment si, vous ne souhaitez plus la tester. Elle devrait déjà être testée et prête à l'emploi pour vos utilisateurs. C'est seulement un cas d'utilisation parmi tant d'autres où vous souhaiterez utiliser le flag `--save-dev`.

Vous rencontrerez beaucoup plus de commandes npm tout au long de votre utilisation. Mais celles-ci seront suffisants pour le moment.

### Exercices :

* installation d'un projet via npm
  * créer un nouveau dossier avec `mkdir <folder_name>`
  * naviguer à l'intérieur du dossier avec `cd <folder_name>`
  * exécuter `npm init -y` ou `npm init`
  * installer un package local tel que React avec `npm install react`
  * regarder à l'intérieur du fichier *package.json* et du dossier *node_modules/*
  * trouver par vous-même comment désinstaller le node package *react*
* lire plus à propos de [npm](https://docs.npmjs.com/)

## Installation

Il y a plusieurs approches pour commencer une application React.

La première consiste à utiliser un CDN. Cela peut sembler plus compliqué que cela l'est. Un CDN est un [content delivery network](https://en.wikipedia.org/wiki/Content_delivery_network). Plusieurs entreprises ont des CDNs qui hébergent des fichiers publiquement pour des personnes puissent les consommer. Ces fichiers peuvent être des bibliothèques telles que React, car après tout, la bibliothèque React empaqueté est seulement un fichier JavaScript *react.js*. Elle peut être hébergée quelque part et vous pouvez la demander au sein de votre application.

Comment utiliser un CDN pour débuter en React? Vous pouvez utiliser la balise inline `<script>` dans votre HTML qui pointe auprès d'une url CDN. Pour débuter en React vous aurez besoin de deux fichiers (bibliothèques) : *react* et *react-dom*.

{title="Code Playground",lang="javascript"}
~~~~~~~~
<script crossorigin src="https://unpkg.com/react@16/umd/react.development.js"></script>
<script crossorigin src="https://unpkg.com/react-dom@16/umd/react-dom.development.js"></script>
~~~~~~~~

Mais pourquoi devrez-vous utiliser un CDN quand vous disposez de npm pour installer des nodes packages tels que React?

Lorsque votre application à un fichier *package.json*, vous pouvez installer *react* et *react-dom* en ligne de commande. Le prérequis est que le dossier est initialisé en tant que projet npm en utilisant `npm init -y` avec un fichier *package.json*. Vous pouvez utiliser plusieurs nodes packages en une seule ligne de commande npm.

{title="Command Line",lang="text"}
~~~~~~~~
npm install react react-dom
~~~~~~~~

Cette approche est souvent utilisée pour ajouter React à une application existante qui est gérée par npm.

Malheureusement ce n'est pas tout. Vous allez être obligéq de vous occuper de [Babel](http://babeljs.io/) pour faire en sorte que votre application soit compatible au JSX (la syntaxe de React) et au JavaScript ES6. Babel transpile votre code ainsi les navigateurs peuvent interpréter le JavaScript ES6 et JSX. Pas tous les navigateurs sont capables d'interpréter la syntaxe. L'installation inclut beaucoup de configuration et d'outillage. Cela peut être accablant pour les nouveaux utilisateurs de React de s'importuner avec toute la configuration.

À cause de cela, Facebook introduit *create-react-app* en tant que solution React zéro configuration. Le prochain chapitre vous montrera comment installer votre application en utilisant cette outil d'initialisation.

### Exercices :

* lire plus à propos des [installations React](https://facebook.github.io/react/docs/installation.html)

## L'installation zéro configuration

In the Road to learn React, vous utiliserez [create-react-app](https://github.com/facebookincubator/create-react-app) pour initialiser votre application. C'est un kit de démarrage orienté avec zéro configuration pour React introduit par Facebook en 2016. [96% des personnes souhaitent le recommander auprès des débutants](https://twitter.com/dan_abramov/status/806985854099062785). Dans *create-react-app*  l'outillage et la configuration évolue en arrière-plan tandis que l'attention est portée sur l'implémentation de l'application.

Pour démarrer, vous devrez installer le package dans votre global node packages. Après quoi, vous l'aurez toujours disponible en ligne de commande pour démarrer de nouvelles applications React.

{title="Command Line",lang="text"}
~~~~~~~~
npm install -g create-react-app
~~~~~~~~


Vous pouvez vérifier la version de *create-react-app* pour vérifier que l'installation a réussi à l'aide de la ligne de commande :

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app --version
*v1.5.1
~~~~~~~~

Maintenant vous pouvez initier votre première application React. Nous l'appelons *hackernews*, mais vous pouvez choisir un autre nom. L'initialisation prend quelques secondes. Ensuite, naviguer simplement à l'intérieur du dossier :

{title="Command Line",lang="text"}
~~~~~~~~
create-react-app hackernews
cd hackernews
~~~~~~~~

Maintenant vous pouvez ouvrir l'application dans votre éditeur. La structure de dossier suivant, ou à quelques variations prêtes dépendant de la version *create-react-app*, devrait vous être présentée :

{title="Folder Structure",lang="text"}
~~~~~~~~
hackernews/
  README.md
  node_modules/
  package.json
  .gitignore
  public/
    favicon.ico
    index.html
    manifest.json
  src/
    App.css
    App.js
    App.test.js
    index.css
    index.js
    logo.svg
    registerServiceWorker.js
~~~~~~~~

Une petite pause sur le dossier et les fichiers. C'est normal si vous ne les comprenez pas tous au début.

* **README.md:** L'extension .md indique que le fichier est un fichier Markdown. Le Markdown est utilisé comme un langage de balisage allégé avec une syntaxe de formatage en texte brut. Plusieurs code source de projets arrivent avec un fichier *README.md* pour donner des instructions initiales à propos du projet. Finalement lors de l'envoi de votre projet auprès d'une plateforme telle que Github, le fichier *README.md* montrera son contenu de manière évidente lorsque vous accédez au dépôt. Comme vous avez utilisé *create-react-app*, votre *README.md* devrait être le même que celui affiché dans le dépôt officiel [dépôt GithHub create-react-app](https://github.com/facebookincubator/create-react-app).

* **node_modules/:** Le dossier a tous les nodes packages qui sont installés via npm. Puisque vous avez utilisé *create-react-app*, iL devrait déjà avoir plusieurs nodes modules installés pour vous. Habituellement, vous ne toucherez jamais à ce dossier, mais seulement installerez et désinstallerez des nodes packages avec npm depuis la ligne de commande.

* **package.json:** Le fichier vous affiche une liste de dépendance de nodes packages et d'autre configurations de projet.

* **.gitignore:** Le fichier indique tous les fichiers et dossiers qui ne devront pas être ajoutés à votre dépôt git distant lors de l'utilisation de git. Ils devraient seulement être présent au sein de votre projet local. Le dossier *node_modules/* en est un exemple d'utilisation. En effet, c'est suffisant de partager le fichier *package.json* avec vos paires pour leur permettre d'installer toutes les dépendances par eux-mêmes sans partager la totalité du dossier de dépendance.

* **public/:** Le dossier possède tous vos fichiers au moment de la construction de votre projet pour la mise en production. Finalement tout votre code écrit dans le dossier *src/* sera empaqueté à l'intérieur de quelques fichiers lors de la construction de votre projet et placé dans le dossier public.

* **manifest.json** et **registerServiceWorker.js:** ne prêtez pas attention à ce que font ces fichiers pour le moment, nous n'en auront pas l'utilité dans ce projet.

Malgré tout, vous n'aurez pas la nécessité de toucher les fichiers et dossiers mentionnés. Pour démarrer la seule chose dont vous aurez besoin est localisé dans le dossier *src/*. L'attention principale se porte sur le fichier *src/App.js* pour implémenter les composants React. Il sera utilisé pour implémenter votre application, mais plus tard vous pourriez vouloir séparer vos composants en plusieurs fichiers tandis que chaque fichier maintient un seul ou quelques composants.

De plus, vous trouverez un fichier *src/App.test.js* pour vos tests et un fichier *src/index.js* en tant que point d'entrée au monde de React. Vous serez amené à connaitre les deux fichiers dans un prochain chapitre. En plus, il y a un fichier *src/index.css* est un fichier *src/App.css* pour styliser votre application de façon générale et vos composants. Les composants arrivent tous avec un fichier de style par défaut lorsque vous les ouvrez.

L'application *create-react-app* est un projet npm. Vous pouvez utiliser npm pour installer et désinstaller des nodes packages de votre projet. De plus, il embarque les scripts npm suivants disponibles en ligne de commande : 

{title="Command Line",lang="text"}
~~~~~~~~
// Lance l'application sur http://localhost:3000
npm start

// Lance les tests
npm test

// Construit l'application pour la mise en production
npm run build
~~~~~~~~

Les scripts sont définis dans votre *package.json*. Votre application React passe-partout est maintenant initialisée. La partie existante arrive avec les exercices pour finalement lancer votre application initialisée au sein du navigateur.

### Exercices :

* `npm start` pour démarrer votre application et visionner l'application dans votre navigateur (vous pouvez arrêter la commande en tapant Ctrl + C)
* lancer le script `npm test` interactif
* lancer le script `npm run build` et vérifier qu'un dossier *build/* a été ajouté à votre projet (vous pouvez de nouveau le supprimer par la suite; noter que le dossier build peut être utilisé plus tard pour [déployer votre application](https://www.robinwieruch.de/deploy-applications-digital-ocean/))
* vous familiarisez avec la structure de dossier
* vous familiarisez avec le contenu des fichiers
* lire plus à propos [des scripts npm et de create-react-app](https://github.com/facebookincubator/create-react-app)

## Introduction au JSX

Maintenant vous allez faire connaissance avec le JSX. C'est une syntaxe utilisée dans React. Comme mentionné avant, *create-react-app* a déjà mis en place un boilerplate d'application pour vous. L'ensemble des fichiers arrivent avec des implémentations par défaut. Il est temps de plonger dans le code source. Le seul fichier que nous manipulerons pour débuter sera le fichier *src/App.js*.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import logo from './logo.svg';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <h1 className="App-title">Welcome to React</h1>
        </header>
        <p className="App-intro">
          To get started, edit <code>src/App.js</code> and save to reload.
        </p>
      </div>
    );
  }
}

export default App;
~~~~~~~~

Ne vous laissez distraire par les déclarations d'import/export et de class. Ce sont des fonctionnalités de JavaScript ES6. À ce titre, nous les réétudierons dans un prochain chapitre.

Dans le fichier vous avez une **React ES6 class component** avec le nom de l'application. C'est une déclaration de composant. Globalement après vous avez déclaré un composant, vous pouvez utiliser ce dernier en tant qu'élément depuis partout dans votre application. Il produira une **instance** de votre **component** ou en d'autres termes: le composant sera instancié.

L'**element** est ce qui est retourné et spécifié dans la méthode `render()`. Les éléments sont la résultant de pourquoi les composants sont créés. C'est essentiel de comprendre la différence entre composant, instance et élément.

Prochainement, vous verrez où l'App component est instancié. Sans quoi vous ne pourriez apprécier le rendu en sortit du navigateur, n'est-ce pas? L'App component est seulement la déclaration, mais non son utilisation. Vous instancierait le composant quelque part dans votre JSX avec `<App />`.

Le contenu dans le bloc de rendu apparaît assez similaire à de l'HTML, mais c'est du JSX. JSX vous permet de mélanger HTML et JavaScript. C'est puissant encore un peu déroutant quand on avait pour habitude de séparer l'HTML du JavaScript. C'est pourquoi, un bon point pour débuter consiste à utiliser de l'HTML basique à l'intérieur de votre JSX. Au tout début, supprimez tout le contenu distrayant du fichier.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
    return (
      <div className="App">
        <h2>Welcome to the Road to learn React</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

Maintenant, vous retournez seulement de l'HTML dans votre méthode `render()` sans JavaScript. Laissez-moi définir le "Welcome to the Road to learn React" en tant que variable. Une variable peut être utilisée dans votre JSX en utilisant les accolades.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    var helloWorld = 'Welcome to the Road to learn React';
# leanpub-end-insert
    return (
      <div className="App">
# leanpub-start-insert
        <h2>{helloWorld}</h2>
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

Cela devrait fonctionner quand vous démarr er votre application à l'aide de la commande `npm start` de nouveau.

Qui plus est vous devriez avoir remarqué l'attribut `className`. Il réfléchit l'attribut HTML standard `class`. Due à des raisons techniques, JSX a remplacé une poignée d'attributs HTML internes. Vous pouvez trouver tous les [attributs HTML supportés au sein de la documentation React](https://facebook.github.io/react/docs/dom-elements.html). Ils suivent tous la convention camelCase. Dans votre apprentissage à React, vous rencontrerez quelques attributs spécifiques JSX supplémentaire.

### Exercices :

* Définir plus de variables et les rendre au sein de votre JSX
  * utiliser un objet complexe pour représenter un utilisateur avec prénom et nom
  * rendre les propriétés de l'utilisateur dans votre JSX
* Lire plus à propos de [JSX](https://facebook.github.io/react/docs/introducing-jsx.html)
* Lire plus à propos des [composants React, éléments et instances](https://facebook.github.io/react/blog/2015/12/18/react-components-elements-and-instances.html)

## ES6 const et let

Je devine que vous avez remarqué que nous avons déclaré la variable `helloWorld` avec une déclaration `var`. JavaScript ES6 arrive avec deux alternatives supplémentaires pour déclarer vos variables : `const` et `let`. En JavaScript ES6, vous trouverez rarement `var` dorénavant.

Une variable déclarée avec `const` ne peut être réassignée ou redéclarée. Il ne peut être muté (changé, modifié). Vous forcez les structures de données immutables en utilisant cela. Une fois la structure de données défini, vous ne pouvez en changer.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// pas permis
const helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

Une variable déclarée avec `let` peut être muté.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// permis
let helloWorld = 'Welcome to the Road to learn React';
helloWorld = 'Bye Bye React';
~~~~~~~~

Vous l'utiliserez quand vous aurez besoin de réassigner une variable.

Cependant, vous devez être vigilant avec `const`. Une variable déclarée avec `const` ne peut pas être modifiée. Mais quand la variable est un tableau d'objet, la valeur contenu peut-être modifié. La valeur contenue n'est pas immutable.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// permis
const helloWorld = {
  text: 'Welcome to the Road to learn React'
};
helloWorld.text = 'Bye Bye React';
~~~~~~~~

Mais quand utiliser chaque déclaration? Il y a différents avis à propos de leur utilisation. Je suggère d'utiliser `const` à chaque fois vous le pouvez. Cela indique que vous souhaiter conserver votre structure de données immutable même si les valeurs des objets et tableaux peuvent être modifiés. Si vous voulez modifier votre variable préconisée `let`.

L'immutabilité est adoptée par React et son écosystème. C'est pourquoi `const` devra être votre choix par défaut quand vous définissez une variable. Encore que, dans les objets complexes les valeurs à l'intérieur peuvent être modifiées. Soyez attentif à propos de ce comportement.

Dans votre application, vous devriez utiliser `const` plutôt `var`.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

class App extends Component {
  render() {
# leanpub-start-insert
    const helloWorld = 'Welcome to the Road to learn React';
# leanpub-end-insert
    return (
      <div className="App">
        <h2>{helloWorld}</h2>
      </div>
    );
  }
}

export default App;
~~~~~~~~

### Exercices :

* lire plus à propos [ES6 const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
* lire plus à propos [ES6 let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
* En savoir plus à propos des structures de données immutables
  * Pourquoi cela a du sens dans la programmation en général
  * Pourquoi sont-elles utilisées dans React et son écosystème

## ReactDOM

Avant de continuer avec l'App component, vous devriez vouloir voir où c'est utilisé. C'est localisé dans votre point d'entrée du monde de React : le fichier *src/index.js*.

{title="src/index.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
~~~~~~~~

Globalement `ReactDOM.render()` utilise un noeud du DOM dans votre HTML pour le remplacer avec votre JSX. C'est comme cela que vous pouvez facilement intégrer React dans toutes applications étrangères. Il n'est pas interdit d'utiliser `ReactDOM.render()` plusieurs fois au travers de votre application. Vous pouvez l'utiliser à différents endroits pour initialiser de la simple syntaxe JSX, un composant React, plusieurs composants React ou toute une application. Mais pour une simple application React vous l'utiliserez seulement une seule fois pour initialiser l'entièreté de votre arbre de composant.

`ReactDOM.render()` attend deux arguments. Le premier argument est du JSX qui sera rendu. Le second argument spécifie la localisation où l'application React prendra place dans votre HTML. Il attend un élément avec un `id='root'`. Vous pouvez dès lors ouvrir votre fichier pour trouver *public/index.html* l'attribut id.

L'implémentation de `ReactDOM.render()` prend déjà votre App component. Cependant, ce serait intéressant de passer du JSX le plus simplifié tant que cela reste du JSX. Cela n'a pas nécessité d'être une instanciation de composant.

{title="Code Playground",lang=javascript}
~~~~~~~~
ReactDOM.render(
  <h1>Hello React World</h1>,
  document.getElementById('root')
);
~~~~~~~~

### Exercices :

* ouvrir *public/index.html* pour voir où les applications React prennent place à l'intérieur de votre HTML
* lire plus à propos [du rendu des éléments dans React](https://facebook.github.io/react/docs/rendering-elements.html)

## Hot Module Replacement

Il y a une chose que vous pouvez faire au sein du fichier *src/index.js* pour améliorer votre expérience de développement en tant que développeur. Mais c'est optionnel et vous ne devrez pas vous en préoccuper au début de votre apprentissage avec React.

*create-react-app* dispose déjà de l'avantage que le navigateur rafraichisse automatiquement la page quand vous modifiez le code source. Essayez en changeant la variable `helloWorld` dans votre fichier *src/App.js*. Le navigateur devrait rafraichir la page. Cependant il y a une meilleure façon de le faire.

Le Hot Module Replacement (HMR) est un outil pour recharger votre application dans le navigateur. Le navigateur n'a pas nécessité de performer un rafraichissement de page. Vous pouvez facilement l'activer dans  create-react-app*. Dans votre *src/index.js*, votre point d'entrée pour React, vous devez ajouter une petite configuration.

{title="src/index.js",lang=javascript}
~~~~~~~~
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);

# leanpub-start-insert
if (module.hot) {
  module.hot.accept();
}
# leanpub-end-insert
~~~~~~~~

C'est tout! Essayez de nouveau de changer la variable `helloWorld` dans votre fichier *src/App.js*. Le navigateur ne dervrait pas performer de rafraichissement de page, mais l'application se recharge et affiche la nouvelle sortie correctement. HMR vient avec de nombreux avantages :

Imaginez-vous êtes en train de debugger votre code avec des déclarations de `console.log()`. Ces déclarations resteront dans votre console développeur, même si vous modifiez votre code, parce que le navigateur ne rafraichit plus la page maintenant. Cela peut être un atout à des fins de debug.

Dans une application grandissant le rafraichissement de page ralenti votre productivité. Vous êtes obligé d'attendre que la page charge. Un rechargement de page peut prendre quelques secondes au sein d'une importante application. HMR repousse ce désavantage.

Le plus grand bénéfice c'est que vous pouvez conserver l'état de l'application avec HMR. Imaginez-vous avez une boîte de dialogue dans votre application avec plusieurs étapes et vous êtes à l'étape 3. Globalement c'est un assistant. Sans HMR vous souhaiteriez changer le code source et votre navigateur rafraichirait la page. Vous devriez rouvrir la boîte de dialogue encore une fois et naviguer de l'étape 1 à 3. Avec HMR votre boîte de dialogue reste ouverte à l'étape 3. Il conserve l'état de l'application même si le code source change. L'application elle-même se recharge, mais pas la page.

### Exercises :

* Modifier votre *src/App.js* code source plusieurs fois et vérifier l'HMR en action
* Regarder les 10 premières minutes du [Live de React: Hot Reloading avec Time Travel](https://www.youtube.com/watch?v=xsSnOQynTHs) par Dan Abramov


## JavaScript complex en JSX

Maintenant retourner à votre App component. Jusqu'à présent, vous rendez quelques variables primitives dans votre JSX. Maintenant vous allez commencer par rendre une liste d'objets. Pour débuter, la liste sera un échantillon de données, mais plus tard vous aller chercher les données depuis une [API](https://www.robinwieruch.de/what-is-an-api-javascript/) externe. Cela sera beaucoup plus passionnant.

Tout d'abord vous devez définir une liste d'objets.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';
import './App.css';

# leanpub-start-insert
const list = [
  {
    title: 'React',
    url: 'https://facebook.github.io/react/',
    author: 'Jordan Walke',
    num_comments: 3,
    points: 4,
    objectID: 0,
  },
  {
    title: 'Redux',
    url: 'https://github.com/reactjs/redux',
    author: 'Dan Abramov, Andrew Clark',
    num_comments: 2,
    points: 5,
    objectID: 1,
  },
];
# leanpub-end-insert

class App extends Component {
  ...
}
~~~~~~~~

L'échantillon de données reflétera les données que nous irons chercher plus tard depuis l'API. Un objet dans la liste possède un titre, une url et un auteur. Qui plus est l'objet dispose d'un identificateur, de points (indique comment un article est populaire) et d'un nombre de commentaires.

Maintenant vous pouvez utiliser la fonctionnalité native `map` dans votre JSX. Cela permet d'itérer sur votre liste d'objets et de les afficher. Encore une fois vous utiliserez les accolades pour encapsuler les expressions JavScript dans votre JSX.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {list.map(function(item) {
          return <div>{item.title}</div>;
        })}
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

Le fait d'utiliser le JavaScript dans l'HTML est assez puissant en JSX. Habituellement, il vous est conseillé d'utiliser `map` pour convertir une liste d'objets en une autre liste d'objets. Cette fois vous utilisez `map` pour convertir une liste d'objets en éléments HTML.

Jusqu'à maintenant, seul le `title` a été affiché à chaque fois. Essayons d'afficher un peu plus de propriétés de l'objet.

{title="src/App.js",lang=javascript}
~~~~~~~~
class App extends Component {
  render() {
    return (
      <div className="App">
# leanpub-start-insert
        {list.map(function(item) {
          return (
            <div>
              <span>
                <a href={item.url}>{item.title}</a>
              </span>
              <span>{item.author}</span>
              <span>{item.num_comments}</span>
              <span>{item.points}</span>
            </div>
          );
        })}
# leanpub-end-insert
      </div>
    );
  }
}

export default App;
~~~~~~~~

Vous pouvez remarquer la fonction map est simplement en ligne dans votre JSX. Chaque propriété de l'objet est affichée dans une balise `<span>`. De plus, la propriété url de l'objet est utilisée dans l'attribut `href` de la balise ancre.

React fera tout le travail pour vous et affichera chaque objet. Mais vous devez ajouter un helper pour que React atteigne son plein potentiel et qu'il améliore ses performances. Vous allez devoir assigner un attribut clé à chaque élément de la liste. Ainsi React sera capable d'identifier les éléments ajoutés, changés et supprimés quand la liste change. L'échantillon d'éléments arrive d'ores et déjà avec un identifieur.

{title="src/App.js",lang=javascript}
~~~~~~~~
{list.map(function(item) {
  return (
# leanpub-start-insert
    <div key={item.objectID}>
# leanpub-end-insert
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  );
})}
~~~~~~~~

Vous devrez vous assurer que l'attribut `key` est un identifiant stable. Ne faites pas l'erreur d'utiliser l'index de l'objet dans un tableau. L'index de tableau n'est pas stable du tout. Par exemple, quand la liste change son ordonnancement, React aura des difficultés à identifier les objets correctement.

{title="src/App.js",lang=javascript}
~~~~~~~~
// ne pas faire cela
{list.map(function(item, key) {
  return (
    <div key={key}>
      ...
    </div>
  );
})}
~~~~~~~~

Vous affichez les deux listes d'objets maintenant. Vous pouvez démarrer votre application, ouvrir votre navigateur et voir les deux objets de la liste affichés.

### Exercices :

* lire plus à propos des [listes et clés de React](https://facebook.github.io/react/docs/lists-and-keys.html)
* Récapitulatif des [fonctionnalités natives et standard du tableau en JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
* Utiliser plus d'expression JavaScript de vous-même en JXS

## ES6 les fonctions fléchées

JavaScript ES6 introduit les fonctions fléchées. Une expression de fonction fléchée est plus courte qu'une expression de fonction.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// function expression
function () { ... }

// arrow function expression
() => { ... }
~~~~~~~~

Mais vous devez rester conscient de ces fonctionnalités. L'un d'eux dispose d'un comportement différent avec l'objet `this`. Une expression de fonction définie toujours son propre objet `this`. Tandis que les expressions de fonction fléchée possèdent encore l'objet `this` du contexte parent. Ne soyez pas désorientés lorsque vous utiliserez `this` à l'intérieur d'une fonction fléchée.

Il y a un autre point intéressant à propos des fonctions fléchées concernant les parenthèses. Vous pouvez supprimer les parenthèses quand la fonction possède seulement un argument, mais vous êtes obligés de les conserver le cas contraire.

{title="Code Playground",lang="javascript"}
~~~~~~~~
// permis
item => { ... }

// permis
(item) => { ... }

// pas permis
item, key => { ... }

// permis
(item, key) => { ... }
~~~~~~~~

Cependant, jetons à coup d'oeil à la fonction `map`. Vous pouvez l'écrire de façon plus concise avec la fonction fléchée ES6.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
{list.map(item => {
# leanpub-end-insert
  return (
    <div key={item.objectID}>
      <span>
        <a href={item.url}>{item.title}</a>
      </span>
      <span>{item.author}</span>
      <span>{item.num_comments}</span>
      <span>{item.points}</span>
    </div>
  );
})}
~~~~~~~~

De plus, vous pouvez supprimer le *block body*, c'est-à-dire les accolades, de la fonction fléchée ES6. Dans un *concise body* un implicite `return` est attaché. Ainsi vous pouvez supprimer la déclaration `return`. Cela s'utilisera assez souvent dans le livre, donc soyez sûre de bien comprendre la différence entre un *block body* et un *concise body* lorsque vous utilisez des fonctions fléchées.

{title="src/App.js",lang=javascript}
~~~~~~~~
# leanpub-start-insert
{list.map(item =>
# leanpub-end-insert
  <div key={item.objectID}>
    <span>
      <a href={item.url}>{item.title}</a>
    </span>
    <span>{item.author}</span>
    <span>{item.num_comments}</span>
    <span>{item.points}</span>
  </div>
# leanpub-start-insert
)}
# leanpub-end-insert
~~~~~~~~

Votre JSX apparaît plus concis et lisible maintenant. Il omet la déclaration de fonction, les accolades et la déclaration du `return`. Au lieu de cela le développeur peut rester concentré sur les détails d'implémentation.

### Exercices :

* lire plus à propos des [fonctions fléchées ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

## ES6 Classes

JavaScript ES6 introduit les classes. Une classe est communément utilisé dans des langages de programmation orientés objet. JavaScript était et est très souple dans ses paradigmes de programmation. Vous pouvez faire de la programmation fonctionnelle et orientée objet côte à côte avec leur cas d'utilisation particulier.

Même si React adhère à la programmation fonctionnelle, par exemple au travers des structures de données immutables, les classes sont aussi utilisées pour déclarer des composants. Elles sont appelées les composants de classe ES6. React mélange les meilleurs des deux paradigmes.

Laissons nous considérer la classe Developer suivante pour examiner une classe JavaScript ES6 sans penser composant.

{title="Code Playground",lang="javascript"}
~~~~~~~~
class Developer {
  constructor(firstname, lastname) {
    this.firstname = firstname;
    this.lastname = lastname;
  }

  getName() {
    return this.firstname + ' ' + this.lastname;
  }
}
~~~~~~~~

Une classe dispose d'un constructeur pour la rendre instanciable. Le constructeur peut prendre plusieurs arguments pour les assigner à l'instance de la classe. De plus, une classe peut définir des fonctions. Du fait que la fonction est associée à une classe, elle est appelée une méthode. Souvent elle est référencée en tant que méthode de classe.

La classe Developer est seulement une déclaration de classe. Vous pouver créer plusieurs instances de classe en l'invoquant. C'est similaire pour une classe ES6 de composant, qui a une déclaration, mais vous devez l'utiliser autres parts pour l'instancier.

Regardons comment vous devez instancier la classe et comment vous pouvez utiliser ses méthodes.

{title="Code Playground",lang="javascript"}
~~~~~~~~
const robin = new Developer('Robin', 'Wieruch');
console.log(robin.getName());
// output: Robin Wieruch
~~~~~~~~

React utilise les classes JavaScript ES6 pour les classes de composants ES6. Vous utilisez d'ores et déjà la classe de composant ES6.

{title="src/App.js",lang=javascript}
~~~~~~~~
import React, { Component } from 'react';

...

class App extends Component {
  render() {
    ...
  }
}
~~~~~~~~

La classe App étend `Component`. Globalement, vous déclarez le composant App, mais il s'étend d'un autre composant. Qu'est-ce que le terme étendre signifie-t-il? En programmation orientée objet vous avez le principe d'héritage. C'est utilisé pour passer des fonctionnalités d'une classe à l'autre.

La classe App étend les fonctionnalités à partir de la classe `Component`. Pour être plus précis, elle hérite des fonctionnalités de la classe `Component`. La classe `Component` est utilisée pour étendre une classe ES6 basique en classe composant ES6. Elle possède toutes les fonctionnalités qu'un composant React a besoin d'avoir. La méthode `render` est une de ces fonctionnalités que vous avez déjà utilisées. Vous apprendrez un peu plus tard les autres méthodes de la classe `Component`.

La classe `Component` encapsule tous les détails d'implémentation du composant React. Permettant aux développeurs d'utiliser des classes en tant que composants dans React.

Les méthodes qu'un React `Component` expose sont l'interface publique. L'une de ces méthodes a besoin d'être surchargé, les autres n'ont pas la nécessité. Vous en apprendrez davantage à propos de cette dernière quand le livre abordera les méthodes du cycle de vie dans un prochain chapitre. La méthode `render()` à besoin d'être surchargé, parce qu'elle définit la sortie d'un `Component` React. Elle doit être définie.

Maintenant vous connaissez les bases autour des classes JavaScript ES6 et de comment elles sont utilisées dans React pour les étendre à des composants. Vous en apprendrez plus au sujet des méthodes de composant quand le livre décrira les méthodes du cycle de vie de React.

### Exercices :

* lire plus à propos des [classes ES6](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes)

{pagebreak}

Vous avez appris à initier votre propre application React! Récapitulons les derniers chapitres :

* React
  * create-react-app démarre une application React.
  * JSX mélange totalement l'HTML et le JavaScript dans le but de définir la sortie des composants React dans leurs méthodes render.
  * Composants, instances et éléments sont différentes choses dans React
  * `ReactDOM.render()` est un point d'entrée pour une application React pour attacher React à l'intérieur du DOM
  * Les fonctionnalités natives de JavaScript peuvent être utilisés en JSX
    * map peut être utilisé pour rendre une liste d'objet en tant qu'éléments HTML
* ES6
  * Les déclarations de variables avec `const` et `let` peuvent être utilisées pour des cas d'utilisation spécifiques
    * Utiliser const par-dessus let dans les applications React
  * Les fonctions fléchées peuvent être utilisées pour conserver vos fonctions concises
  * Les classes sont utilisées pour définir les composants dans React en les étendant

Il peut être intéressant de faire une pause à ce niveau-ci. Intérioriser les acquis et les appliquer de façon autonome.

Vous pouvez trouver le code source dans le [dépôt officiel](https://github.com/rwieruch/hackernews-client/tree/4.1).
