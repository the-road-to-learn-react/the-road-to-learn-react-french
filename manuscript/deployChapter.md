# Dernière étape pour la mise en production

Les derniers chapitres vous montreront comment déployer votre application en production. Vous utiliserez le service d'hébergement gratuit Heroku. Lors du déploiement de votre application, vous en apprendrez plus sur *create-react-app*.

## Eject

L'étape suivante et les connaissances liées ne sont **pas nécessaires** pour déployer votre application en production. Cependant je souhaite vous l'expliquer. *create-react-app* arrive avec une fonctionnalité qui le rend extensible mais aussi empêche une clientèle captive. Une clientèle captive se produit habituellement lorsque vous achetez une technologie mais qu'il n'y a pas de trappe d'évacuation pour son usage futur. Heureusement dans *create-react-app* vous disposez d'une telle trappe à l'aide d'**eject**.

Dans votre *package.json* vous trouverez les scripts pour *start*, *test* et *build* votre application. Le dernier script est *eject*. Vous pouvez l'essayer, mais il n'y a aucun moyen de faire chemin inverse. **C'est une opération à sens unique. Une fois que vous éjectez, vous ne pouvez revenir en arrière!** si vous venez juste de démarrer l'apprentissage de React, cela n'a aucun intérêt de quitter l'environnement commode de *create-react-app*.

Si vous souhaitez lancer `npm run eject`, la commande fera une copie de toute la configuration et des dépendances vers votre *package.json* et un nouveau dossier *config/*. Vous souhaiteriez convertir le projet complet dans une installation customisée comprenant l'outillage incluant Babel et Webpack. Après quoi, vous possèderiez un contrôle absolu sur tous ces outils.

La documentation officielle dit que *create-react-app* est adaptée pour des projets de petite à moyenne taille. Vous ne devez pas vous sentir obligé d'utiliser la commande "eject". 

### Exercices :

* lire plus à propos d'[eject](https://github.com/facebookincubator/create-react-app#converting-to-a-custom-setup)

## Déployer votre application

Au bout du compte, aucune application ne devrait rester en local. Vous souhaitez la mettre en ligne. Heroku est un PaaS (Platform as a Service) où vous pouvez héberger votre application. Ils offrent une intégration transparente avec React. Pour être plus précis : il est possible de déployer une application *create-react-app* en quelques minutes. C'est un déploiement avec zéro-configuration qui suit la philosophie de *create-react-app*.

Vous avez besoin de remplir deux prérequis avant de pouvoir déployer votre application sur Heroku :

* installer le [CLI d'Heroku](https://devcenter.heroku.com/articles/heroku-command-line)
* créer un [ compte Heroku gratuit](https://www.heroku.com/)

Si vous avez installé Homebrew, vous pouvez installer le CLI d'Heroku en ligne de commande :

{title="Command Line",lang="text"}
~~~~~~~~
brew update
brew install heroku-toolbelt
~~~~~~~~

Maintenant vous pouvez utiliser Git et le CLI d'Heroku pour déployer votre application.

{title="Command Line",lang="text"}
~~~~~~~~
git init
heroku create -b https://github.com/mars/create-react-app-buildpack.git
git add .
git commit -m "react-create-app on Heroku"
git push heroku master
heroku open
~~~~~~~~

C'est tout. Maintenant, j'espère que votre application a démarré et est en cours d'exécution. Si vous rencontrez des problèmes vous pouvez vérifier les ressources suivantes :

* [Git et GitHub Essentials](https://www.robinwieruch.de/git-essential-commands/)
* [Déploiement de React avec zéro-configuration](https://blog.heroku.com/deploying-react-with-zero-configuration)
* [Heroku Buildpack pour create-react-app](https://github.com/mars/create-react-app-buildpack)
