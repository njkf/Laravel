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
