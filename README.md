# symfony-project-setup
Common instructions to pull and setup a Symfony/MySql porject

## Local setup

1) Setup PHP >= 8.1 and extensions

PHP version and extensions are specified in the `composer.json` file

```bash
// For MacOX
brew install php pkg-config imagemagick
pecl install imagick fileinfo iconv
// For Linux (depends on the package manger fo course)
sudo apt update && sudo apt install --no-install-recommends php8.1
sudo apt-get install -y php8.1-cli php8.1-common php8.1-mysql php8.1-zip php8.1-gd php8.1-mbstring php8.1-curl php8.1-xml php8.1-bcmath php8.1-imagick php8.1-fileinfo php8.1-iconv
pecl install imagick fileinfo iconv
```

2) Install Composer globally

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
```

3) Install Symfony binary

https://symfony.com/download


```bash
// For MacOX
brew install symfony-cli/tap/symfony-cli
// For Linux
curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | sudo -E bash
sudo apt install symfony-cli
```


4) Install MySqL

```bash
// For MacOX
brew instal mySql
// For Linux
sudo apt install mysql-server
sudo mysql_secure_installation
```

5) Copy keys from ./tests/keys to ./var/oauth

```bash
cp ./tests/keys/* ./var/oauth
```

6) Create a MySql database, user, and grant

```bash
mysql
CREATE USER '{db_user}'@'localhost' IDENTIFIED BY '{db_pwd}'; CREATE DATABASE {db_name}; GRANT ALL PRIVILEGES ON {db_name} . * TO '{db_user}'@'localhost';
```

7) Run migrations

```bash
symfony console docrine:migrations:migrate
```

8) Install Node, Npm, Yarn

```bash
// For MacOX
brew instal node
// For Linux
sudo apt install nodejs npm
// For both, install Yarn
npm i -g yarn
```

9) Install Dependencies

```bash
composer install
yarn
```

10) Create the first User

You can either

* Write a custom command such as this one https://github.com/symfony/demo/blob/main/src/Command/AddUserCommand.php
* Or create it straight into the database https://symfony.com/doc/6.2/the-fast-track/en/15-security.html#generating-a-password-for-the-admin-user

11) Install certificates for local SSL

```bash
symfony server:ca:install
```

12) Setup tests 

```bash
mysql
CREATE DATABASE {test_db_name}; GRANT ALL PRIVILEGES ON {test_db_name} . * TO '{db_user}'@'localhost';
symfony console doctrine:migrations:migrate --env=test
php ./vendor/bin/simple-phpunit tests
```

## Deploy to a real server

You can deploy to almost any server, any OS, on any web server, on a IAAS or a PAAS infrastructure, on a raw VM or on a dockerized infrastructure.
The easiest way is, according to me, to use Symfony Cloud (Platform.sh) : https://symfony.com/cloud/
For more information about deployment of a Symfony Application, and web server configuration, please follow the official symfony documentation"

* https://symfony.com/doc/current/deployment.html
* https://symfony.com/doc/current/setup/web_server_configuration.html
