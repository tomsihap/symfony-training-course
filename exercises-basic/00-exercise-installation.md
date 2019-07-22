# Installation

Première chose à faire, comme toujours, ouvrir la [documentation](https://symfony.com/doc/current/setup.html) !

L'objectif de cet exercice est d'installer et configurer un environnement de travail avec Symfony.

## PHP

Pour toute la suite des exercices, il est important d'avoir PHP installé sur votre machine (via Wamp, Mamp ou autres...) et qu'il soit accessible en variable d'environnement.

Pour tester s'il est accessible ouvrez un terminal/console et tapez:

```bash
php -v
```

Si quelque chose comme ci-dessous apparait, c'est bon !

```bash
PHP 7.1.1 (cli) (built: Feb 13 2017 10:05:49) ( NTS )
```

dans le cas contraire, il faut procéder à quelques manipulations

> Sous Windows
>
> Allez dans Panneau de configuration puis Système et sécurité puis Sécurité/Système. Ici se trouve un bouton "Paramètre de système avancé".
> Cliquez sur variables d’environnement.
> Il vous faut maintenant ajouter le chemin d’accès vers votre exécutable PHP (php.exe) à la variable d’environnement PATH.

> Sous MacOS / Linux
>
> Normalement, PHP est déjà présent sur l'OS. Si vous utilisez un serveur local de type Mamp ou autre,
> il est possible que vous ayez 2 versions de PHP sur votre machine. Pensez à bien tout maintenir à jour
> pour éviter les conflits.

## Symfony

Ouvrez un terminal/console et placez vous dans le futur dossier parent de votre projet.

Suivez ensuite les étapes de la documentation, [*Creating Symfony Applications*](https://symfony.com/doc/current/setup.html), pour créer une nouvelle application Symfony.
Nommez la comme vous le souhaitez (`firstProject` par exemple). Nous travaillerons avec la dernière version stable.

Utilisez la commande suivante :

```bash
composer create-project symfony/website-skeleton firstProject
```
Il s'agit du Framework en lui-même et de l'ensemble de ses composants.

La commande vous indique tous les bundles qu'elle vient d'installer.

Via la console, placez vous maintenant dans le dossier nouvellement créé grâce à :

```bash
cd firstProject
```

Tapez la commande suivante pour voir si le projet est correctement installé:

```bash
php bin/console
```

Si la liste des commandes proposées par Symfony apparaissent, vous pouvez passer à la suite.

## Ouvrir le projet dans l'éditeur de code

Le projet maintenant créé, ouvrez-le dans l'éditeur de code (VSCode, PHPStorm...). Il est très important de s'assurer que le bon dossier est ouvert et non pas un dossier parent, en effet la console de l'éditeur de code étant située dans le dossier ouvert, les commandes (git, composer, symfony...) doivent être exécutées dans le bon dossier.

## Extensions VSCode

1. Désinstallez PHP Intellisense si l'extension est installée
2. Installez l'extension PHP Intelephense et redémarrez VSCode

