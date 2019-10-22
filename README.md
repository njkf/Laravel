# Doc Laravel


# **Environnement Laravel**

**Sommaire**

## Installations requises

* Installer Visual Code Studio : https://code.visualstudio.com/download
* Installer et utiliser composer :
    * Avec Windows : https://getcomposer.org/Composer-Setup.exe
    * Installation manuelle : https://getcomposer.org/doc/faqs/how-to-install-composer-programmatically.md
* Installer un serveur web et une base de donnée :
    * Je propose directement xampp qui fournira les deux : https://www.apachefriends.org/fr/download.html (à la racine de votre disque)
    * Ou installer MySQLServer : 
* Installer PHP : https://www.php.net/manual/fr/install.php
* Installer Homestead : https://apical.xyz/fiches/travailler_avec_laravel/installation_de_homestead_sous_windows_pour_developper_en_laravel

Prêt pour débuter Laravel

## Laravel

Tout d'abord il va nous falloir installer les pré-requis via l'installateur laravel pour pouvoir créer un nouveau projet.
Si vous utilisez xampp, installer laravel parmis ces dossiers.

```composer global require laravel/installer```

```laravel new project```

Ou en utilisant directement le composer

```composer create-project --prefer-dist laravel/laravel project```

Allumer xampp, connectez votre serveur mysql et apache. 

En lançant dans votre composer ou votre invité de commande ```php artisan serve```, vous lancez votre serveur local que vous pouvez afficher avec : `http://localhost:8000`

A ce stade, vous pouvez naviguer dans les dossiers laravel afin de trouver le fichier php de votre local server


## Users

On va commencer par créer un UserTableSeeder pour remplir la base de données de 100 utilisateurs.
Pour générer le seeder, on va executer la commande: make:seeder Artisan.
Tout les seeders généré par le framework vont être automatiquement placés  dans le dossier database/seeds.
    
    ```php artisan make:seeder UsersTableSeeder```
    
On va remplir le fichier DatabaseSeeder comme ceci. On utilise la méthode run pour appeller UsersTableSeeder :

```
<?php

use Illuminate\Database\Seeder;

class DatabaseSeeder extends Seeder
{
    /**
     * Seed the application's database.
     *
     * @return void
     */
    public function run()
    {
        $this->call(UsersTableSeeder::class);
    }
}
```

On va ensuite utiliser run dans UsersTableSeeder pour créer les 100 utilisateurs:
```
public function run(){
    factory(App\User::class, 100)->create();
}
```

Une fois que le seeder est écrit, on utilise la commande dump-autoload de composer :
```
composer dump-autoload
```

On va ensuite remplir la base de donnée :
```
php artisan db:seed --class=UsersTableSeeder
```

Ici, `--class` est optionnel, il est là pour spécifier qu'une classe de seeder a été initialisé et a été rajouté.

Puis, si besoin, pour completement reconstruire la base de donnée :
```
php artisan migrate:fresh --seed
```

Maintenant nous allons associer les users avec les rôles et les permissions, rentrez cette commande pour que composer configure automatiquement la création des permissions
```
composer require spatie/laravel-permission
```

Sinon en rajoutant la ligne de code ci-dessous dans `config/app.php` :
```
'providers' => [
    // ...
    Spatie\Permission\PermissionServiceProvider::class,
];
```

Nous pouvons maintenant exécuter la migration avec : 
```
php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider" --tag="migrations"
```

Ou en allant dans ce dépôt GitHub : `https://github.com/spatie/laravel-permission/blob/master/database/migrations/create_permission_tables.php.stub`

A partir de là, on peut configurer les UUID's et GUID's pour les users sauf qu'on en a pas besoin ici donc on fera sans. Après avoir publié la miragtion, il faut créer les tables de rôles et de permissions :
```php artisan migrate```

Et enfin publiez le tout avec ```php artisan vendor:publish --provider="Spatie\Permission\PermissionServiceProvider" --tag="config"```

Vous avez accès à un nouveau fichier dans `config/permission.php` où un model est créé. Vous pouvez ou non modifier le fichier, cependant je vous recommande de ne pas le modifier pour l'instant.

## Seeders

Ici, vous aurez une explication pour créer et utiliser les seeders de laravel.
Avant de commencer vous pouvez créer le fichier RolesTableSeeder (`database/seeds/RolesTableSeeder`) où les rôles de vos seeders seront décrits dedans.

Maintenant en utilisant les permissions de Spatie, il faut annoncé que les users auront un rôle d'attribuer.
