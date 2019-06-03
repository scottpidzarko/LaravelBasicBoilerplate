## Laravel 5.7 Basic Boilerplate

[![Latest Stable Version](https://poser.pugx.org/rappasoft/laravel-5-boilerplate/v/stable)](https://packagist.org/packages/rappasoft/laravel-5-boilerplate)
[![Latest Unstable Version](https://poser.pugx.org/rappasoft/laravel-5-boilerplate/v/unstable)](https://packagist.org/packages/rappasoft/laravel-5-boilerplate)

<!---
TODO Update to my own StyleCI and CircleCI badges
-->

[![StyleCI](https://styleci.io/repos/30171828/shield?style=plastic)](https://styleci.io/repos/30171828/shield?style=plastic)
[![CircleCI](https://circleci.com/gh/rappasoft/laravel-5-boilerplate/tree/master.svg?style=svg)](https://circleci.com/gh/rappasoft/laravel-5-boilerplate/tree/master)

### Features

* Access Control
  * Register/Login/Logout/Password Reset
  * Third party login (Github/Facebook/Twitter/Google/Linked In/BitBucket)
  * Account Confirmation By E-mail
  * Resend Confirmation E-mail
  * Option for Manual Account Confirmation by Admin
  * Login Throttling
  * Enable/Disable Registration
  * Force Single Session
  * Clear User Session
  * Configurable Password History
  * Administrator Management
    * User Index
    * Activate/Deactivate Users
    * Soft & Permanently Delete Users
    * Resend User Confirmation E-mails
    * Change Users Password
    * Create/Manage Roles
    * Manage Users Roles/Permissions
    * "Login As" User
    * Kill User Session
* Default Responsive Layout
* Frontend and Backend Controllers
* User Dashboard
* Administration Dashboard with CoreUI
* Namespaced Routes
* Default Forms Converted to Form Helper Methods
* Master Layout Files with common sections
* Versioned CSS/JS Files
* Helper functions
* Javascript/jQuery Snippets
* Bootstrap 4
* Font Awesome 5
* Global Messages/Exception Handling
* Socialite Integration
* Active Menu
* ARCANEDEV Log Viewer
* Dynamic Breadcrumbs
* Localization with RTL support in 15+ languages so far.
* Gravatar
* Laravel Debugbar
* Event subscribers
* Google reCaptcha
* Vue
* Standards
  * PSR-2
  * Clean Controllers
  * Repository/Contract Implementations
  * Request Classes
  * Events/Handlers
  * Entire application split between frontend/backend
  * Localization Throughout

### Required packages for Development

This project uses laradock for it's development environment. Laradock is a git submodule of this project, so it will be cloned when you clone this repository.

This project requires the following, which come with Laradock already:

[Composer](https://getcomposer.org)
[NPM](https://www.npmjs.com/get-npm)

MySQL or your favourite SQL engine. Instructions below for MySQL, but Laradock supports MSSQL, MariaDB, Postgre SQL, etc. If you choose anything other than MariaDB/MySQL you're on your own.

### Optional

[Yarn (highly recommended!)](https://yarnpkg.com/en/docs/install)

### Installation/Setup for Development

This guide is based on http://laradock.io/guides/#Digital-Ocean and https://www.digitalocean.com/community/tutorials/how-to-set-up-laravel-nginx-and-mysql-with-docker-compose and Rappasoft’s setup guide for their boilerplate

1. Download

```console
git clone https://github.com/scottpidzarko/LaravelBasicBoilerplate.git
```

or if you have an SSH Key setup:

```console
git clone git@github.com:scottpidzarko/LaravelBasicBoilerplate.git
```

After the file has downloaded, enter the project directories' laradock/ folder:

```console
cd LaravelBasicBoilerplate/laradock
```

Pull the laradock folder:

```console
git submodule update --init --recursive
```

2. Environment File - Laradock
This package ships with a .env.example file in the laradock/ folder.

Duplicate this file, and rename to just .env;

```console
cp .env.example .env
```

Note: If you cannot see the file, make sure you have hidden files shown on your system.

Use your favourite text editor to edit the file. We're going to alter a few lines. It's a somewhat large config file, so searching for the variable names we are going to set will be helpful to speed up this process:

App code path: this is where your Laravel project root folder is. If you're following this guide exactly, it will be one level up from the laradock folder
```console
APP_CODE_PATH_HOST=../
```

Data path for your host: this is where any data persistent to the virtual machine stays - your database and machine logs get put here. Later you can choose not to save the database, but for now it goes here.

```console
DATA_PATH_HOST= ~/.laradock/data

Other more self-explanatory variables to set:

```console
COMPOSE_PATH_SEPERATOR=: #or ; on Windows
WORKSPACE_TIMEZONE=America/Vancouver #(or whatever yours is)
WORKSPACE_INSTALL_NPM_GULP=false
PHP_FPM_INSTALL_PHPREDIS=false
WORKSPACE_INSTALL_PHPREDIS=false
```


Make sure it’s mysql 8.0.3
Change docker host IP to your host computers ipv4 ADDR

Check docker_sync strategy for your OS
Check compose_convert_windows paths for your OS
Check apache_document_root -> should be /var/www/public
Make sure mysql creds are matching what you want



3. Environment File - Project

This package also ships with a .env.example file in the root folder of the project. Like before, duplicate and rename the file:

```console
cp .env.example .env
```

Use your favourite text editor to edit this file as well. It's a little smaller.

Enter your MySQL credentials you set in the laradock .env file:

```console
DB_CONNECTION=mysql
#keep this as 'mysql' if you're running with laradock's docker-compose
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=default #whatever you defined
DB_USERNAME=default #whatever you defined
DB_PASSWORD=secret #whatever you defined
```

Change the other environment variables

```console
APP_KEY=  #LEAVE BLANK - it will be filled later on in this process with Artisan.
APP_TIMEZONE=America/Vancouver #whatever you defined in laradock .env file
APP_LOCALE=en #default is fine for most websites unless you are developing for an end-user outside North America
APP_LOCALE_PHP=en_US #default is fine for most people
APP_DEBUG=true #false on production
```
Every other default should be OK to leave alone for most users, or can be tweaked later

4. Spin up the docker virtual machine

Once you're sure you have your config files set, lets build and start our docker container using _docker compose_:

```console
docker-compose up -d mysql apache2
```

The -d flag starts the machine in the background once they are built. This step can take a while so go have a coffee or something.

You can see your docker container with

```console
docker ps
```

You should see an apache2 machine, a mysql machine, your workspace machine all linked together with the docker network machine like this:

<!--TODO: Insert picture -->

If something went wrong in the build and the machine isn't there, or if you need to change something in your laradock.env and have to rebuild, just run:

```console
docker-compose build apache2 mysql
```
Then after your build is successful, re-run:

```console
docker-compose up -d apache2 MySQL
```

3. Composer

Composer manages the Laravel Project's dependencies.

login
```console
docker-compose exec workspace bash
```

You are now in your workspace machine, where the php process lives. You will be dropped into your web root:

```
root@docker-machine:/var/www $ composer install
```

This step can take a bit but it's not as long as building the docker image. After it's successful move on to the next step.

4. Artisan Commands

We are going to so is set the key that Laravel will use when doing encryption.

```console
root@docker-machine:/var/www $ php artisan key:generate
```

You should see a green message stating your key was successfully generated. As well as you should see the APP_KEY variable in your .env file reflected.

It's time to see if your database credentials are correct.

We are going to run the built in migrations to create the database tables:

```console
foo@bar:~$ php artisan migrate
```

You should see a message for each table migrated, if you don't and see errors, than your credentials are most likely not correct.

5. Set local directory permissions:

On Windows, in Powershell:

First exit the docker virtual environment
/var/www# exit

PS LaravelBasicBoilerplate\laradock> cd ..

PS LaravelBasicBoilerplate\laradock> .\storage\ /grant Users:F

PS LaravelBasicBoilerplate\laradock> .\bootstrap\cache\ /grant Users:F

On Linux, in bash:

$ cd .. && chmod 777 -R ./storage ./bootstrap/cache

6. NPM/Yarn
<!-- TODO determine if I have to run this step -->

In order to install the Javascript packages for frontend development, you will need the Node Package Manager, and optionally the Yarn Package Manager by Facebook (Recommended)

If you only have NPM installed you have to run this command from the root of the project:

```console
root@docker-machine:~$ npm install
```

If you have Yarn installed, run this instead from the root of the project:

```console
foo@bar:~$ yarn
```

#

We are now going to set the administrator account information. To do this you need to navigate to this file and change the name/email/password of the Administrator account.

You can delete the other dummy users, but do not delete the administrator account or you will not be able to access the backend.

Now seed the database with:

```console
foo@bar:~$ php artisan db:seed
```

You should get a message for each file seeded, you should see the information in your database tables.

7. NPM Run '\*'
Now that you have the database tables and default rows, you need to build the styles and scripts.

These files are generated using Laravel Mix, which is a wrapper around many tools, and works off the webpack.mix.js in the root of the project.

You can build with:

```console
foo@bar:~$ npm run <command>
```

The available commands are listed at the top of the package.json file under the 'scripts' key.

You will see a lot of information flash on the screen and then be provided with a table at the end explaining what was compiled and where the files live.

At this point you are done, you should be able to hit the project in your local browser and see the project, as well as be able to log in with the administrator and view the backend.

8. PHPUnit
After your project is installed, make sure you run the test suite to make sure all of the parts are working correctly. From the root of your project run:

```console
foo@bar:~$ phpunit
```

You will see a dot(.) appear for each of the hundreds of tests, and then be provided with the amount of passing tests at the end. There should be no failures with a fresh install.

9. Storage:link
After your project is installed you must run this command to link your public storage folder for user avatar uploads:

```console
foo@bar:~$  php artisan storage:link
```

10. Login
After your project is installed and you can access it in a browser, click the login button on the right of the navigation bar.

The administrator credentials are:

**Username**: admin@admin.com

**Password**: secret

You will be automatically redirected to the backend. If you changed these values in the seeder prior, then obviously use the ones you updated to.

### Deployment

#### Digital Ocean

I use Digital Ocean and Docker for Deployment.
