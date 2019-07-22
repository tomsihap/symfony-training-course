# Routing et controllers

Première chose à faire, comme toujours, ouvrir la [documentation](https://symfony.com/doc/current/controller.html) !

L'objectif de cet exercice est de comprendre le fonctionnement du routing, des controllers et de la création de pages.

> **Une route** : c'est le chemin/URL vers votre page (`/articles`, `/users/1`, `/products/add`...)

> **Un controller** : c'est la fonction PHP qui sera exécutée lorsqu'on arrivera sur la route associée. Cette fonction doit toujours retourner un résultat, l'objet `Response` de Symfony. Cette réponse peut contenir une view (HTML simple ou avec Twig), du JSON, du XML, une redirection, une erreur 404...

> **Une view** : elle correspond à l'affichage de votre page.

> **Important !** Dans le cadre du cours, gardez bien en tête que **1 route = 1 controller = 1 view**. Donc pour chaque route, pensez bien à créer une nouvelle action dans le controller et une nouvelle vue.
Plus tard, on verra que ce n'est pas toujours le cas mais pour la compréhension au départ c'est important.

## Controller et route de base

> Nous travaillons dans le dossier **src/**. C'est ici que se trouve votre code.

Effectuez la commande suivante :

```bash
php bin/console make:controller
```

Répondez à la question demandée, et nommez ainsi votre controller (par exemple : `ArticlesController`).

Validez.

> **Note :** Dans tous les cas où vous exécutez une commande Symfony, vous pouvez quitter l'invite de commandes avec le raccourci `ctrl+c`.

Deux nouveaux fichiers viennent d'être créé `src/controller/ArticlesController` et `templates/articles/index.html.twig`

Le controller possède déjà une route :

```
/**
 * @Route("/articles", name="default")
 */
public function index()
{
    return $this->render('articles/index.html.twig', [
        'controller_name' => 'ArticlesController',
    ]);
}
```

## Première route

Créez une première action de controller simple en ajoutant à la suite :

```php
    /**
     * @Route("hello", name="route_hello")
     */
    public function hello()
    {
        return new Response('Hello world');
    }
```

1. Nous avons créé ici une route `@Route("hello")` : c'est à dire qu'on créée l'URL `localhost:8000/hello`.
2. Lorsque l'on va sur cette URL, l'action `hello()` sera exécutée
3. Cette action retourne un objet de type `Response`, avec en paramètre une string `'Hello world'`.

> **Très important :** Vous remarquerez que lors de la saisie du mot `Response`, un cadre d'autocomplétion est apparu : en effet, plusieurs composants `Response` existent, mais un seul nous intéresse : `Symfony\Component\HttpFoundation\Response`.

> Pour être certain que le bon composant est utilisé, regardez si la ligne `use Symfony\Component\HttpFoundation\Response;` existe en haut du fichier.
>
> Si un autre composant finissant par `...\Response` est présent ou si la ligne n'est pas présente du tout, c'est que l'autocomplétion n'a pas fonctionné. Effacez et réécrivez `Response` de sorte à ce que cela fonctionne.

> Cette pratique sera utilisée **à chaque fois** que l'on utilisera une classe : c'est à dire tous les mots clés qui commencent par une lettre majuscule !


### Testez la route

Pour tester la route, il va falloir lancer le serveur. Ouvrez une console (`ctrl+ù` en général) et saisissez :
`php bin/console server:run`

> **Attention** : Si vous n'avez pas exactement ouvert le dossier du projet (mais un dossier parent par exemple), la commande ne fonctionnera pas.

S'il y a des erreurs dans le code, la console vous l'indiquera, il faudra modifier et enregistrer les fichiers de sorte à corriger les erreurs, puis recommencer la commande.

Une fois le serveur lancé, vous pouvez accéder aux routes créés, par exemple ici : `http://localhost:8000/hello`.

## Seconde route

Créez une seconde action de controller en ajoutant à la suite :

```php
    /**
     * @Route("nombre/aleatoire", name="nombre_aleatoire")
     */
    public function luckyNumber()
    {
        $luckyNumber = rand(0, 100);
        return new Response($luckyNumber);
    }
```

1. Nous avons créé ici une route `@Route("nombre/aleatoire")` : c'est à dire qu'on créée l'URL `localhost:8000/nombre/aleatoire`.
2. Lorsque l'on va sur cette URL, l'action `luckyNumber()` sera exécutée
3. Cette action génère un nombre aléatoire `rand(0, 100)`...
4. ... et retourne un objet de type `Response`, avec en paramètre le nombre aléatoire.


## Troisième route (Twig)

En reprenant la **seconde route**, remplacez la ligne `return new Response()` par :

```php
    return $this->render('pages/number.html.twig', [
        'number' => $luckyNumber
    ]);
```

1. On retourne  `$this->render()` qui en fait retourne un objet `Response` avec une vue en paramètres : il nous permet d'appeler une vue
2. On appelle la vue `pages/number.html.twig`
3. On passe à la vue une variable `number` qui contient la variable `$luckyNumber`.

Créez à présent le template associé : `/templates/pages/number.html.twig`

```twig
{% extends 'base.html.twig' %}

{% block body %}
    Voici le nombre choisi : {{number}}
{% endblock %}
```

Accédez maintenant à l'URL [http://localhost:8000/nombre/aleatoire/](http://localhost:8080/nombre/aleatoire)

## Quatrième route: avec un paramètre
Maintenant, ajoutez un paramètre à l'url qui correspond au nombre max affiché aléatoirement.

Créez cette route à la suite :

```php

    /**
     * @Route("/nombre/aleatoire/max/{maximum}", name="nombre_aleatoire_max")
     */
    public function luckyNumberMax($maximum)
    {
        $luckyNumber = mt_rand(0, $maximum);

        return $this->render('pages/number.html.twig', [
            'number' => $luckyNumber
        ]);
    }

    // [...]
```

Pour tester, allez sur : `localhost:8000/nombre/aleatoire/max/42` par exemple.

## Bonus : ajouter une contrainte sur le paramètre

Etape suivante, ajoutez une contrainte pour que ce paramètre soit obligatoirement un nombre. Pas de code en plus dans le controller ici, tout se fait en annotation, fouillez dans la doc pour trouver !

Pour tester, allez sur : `localhost:8000/nombre/aleatoire/max/seize` : vous devriez avoir une erreur.

## Bonus : donner une valeur par défaut au paramètre
Dernière étape, faire en sorte que ce paramètre soit optionnel et qu'il ait une valeur par défaut. Testez ensuite la route

Pour tester : allez sur `localhost:8000/nombre/aleatoire/max/` : la route devrait fonctionner sans paramètre dans l'URL.

## Routing avancé

Si vous ne l'aviez pas encore ouverte voici la doc concernant le [routing](https://symfony.com/doc/3.4/routing.html).

> Lors que vous créez un nouveau controller, il y a une nomenclature précise à respecter. Le nom de la fonction doit toujours être suffixée (finir) par "Action".

1. Créez une nouvelle action dans le `controller` avec comme route : `/blog`. Elle retournera la vue `pages/blog.html.twig`.

2. Ajoutez 3 paramètres à l'URL: _locale, year, title. Par exemple : `localhost:8000/blog/fr/2018/comment-faire-manger-des-epinards-aux-enfants`, soit en décomposé : `localhost:8000/_locale/year/title`

3. Dans la vue `blog.html.twig`, affichez une phrase du genre: "Voici l'article $title, de l'année $year, en langue $_locale".

4. Ajoutez les contraintes suivantes:
- _locale ne peut être que 'fr' ou 'en'
- year doit être un nombre à 4 chiffre
- title n'est composé que de lettres, chiffres ou -

5. Dans la vue, si la _locale est 'en', afficher le texte en anglais. (Indice: regardez comment gérer les `if/else` avec Twig)

6. Bonus: faire en sorte que _locale soit optionnelle et que sa valeur par défaut soit 'fr'
