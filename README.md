# symfony-project-setup
Common instructions to pull and setup a Symfony/MySQL project

## Local Setup

### Installation of Dependencies

#### 1. Install PHP >= 8.4 and Extensions

The required PHP version and extensions are specified in the composer.json file.

To install PHP 8.4 and the necessary extensions:
- **For Linux:**
  1. Add the repository for PHP 8.4:
    ```bash
    sudo add-apt-repository ppa:ondrej/php
    sudo apt update
    ```
  2. Install PHP 8.4 with extensions:
    ```bash
    sudo apt install php8.4 php8.4-cli php8.4-fpm php8.4-common \
      php8.4-bcmath php8.4-bz2 php8.4-curl php8.4-dom php8.4-exif \
      php8.4-fileinfo php8.4-gd php8.4-iconv php8.4-intl php8.4-mbstring \
      php8.4-mysql php8.4-opcache php8.4-pdo php8.4-zip php8.4-imagick
    ```
  3. Switch to the new PHP 8.4 version by default :
    ```bash
    sudo update-alternatives --set php /usr/bin/php8.4
    ```
  4. Optional: restart FPM (if using Symfony CLI or Nginx):
    ```bash
    sudo systemctl stop php8.3-fpm
    sudo systemctl restart php8.4-fpm
    sudo systemctl enable php8.4-fpm
    ```
  5. Optional: restart Apache (if using Apache):
    ```bash
    sudo a2dismod php8.3
    sudo a2enmod php8.4
    sudo systemctl restart apache2
    ```

- **For macOS:**
    ```bash
    brew tap shivammathur/php
    brew install php@8.4
    brew install imagemagick
    ```

#### 2. Install Composer Globally

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '55ce33d7678c5a611085589f1f3ddf8b3c52d662cd01d4ba75c0ee0459970c2200a51f492d557530c71c15d8dba01eae') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php
php -r "unlink('composer-setup.php');"
sudo mv composer.phar /usr/local/bin/composer
```

Or update an existing Composer to the latest stable version :

```bash
composer self-update
```

#### 3. Install Symfony Binary

- **For macOS:**
    ```bash
    brew install symfony-cli/tap/symfony-cli
    ```

- **For Linux:**
    ```bash
    wget https://get.symfony.com/cli/installer -O - | bash
    sudo mv ~/.symfony5/bin/symfony /usr/local/bin/symfony
    ```

### Database Configuration

#### 4. Install MySQL

- **For Linux:**
    ```bash
    sudo apt install mysql-server
    sudo mysql_secure_installation
    ```

- **For macOS:**
    ```bash
    brew install mysql
    brew services start mysql
    ```

#### 5. Copy OAuth Keys

Note: Before copying the keys, create the oauth folder manually:
```bash
mkdir -p ./var/oauth
cp ./tests/keys/* ./var/oauth
```

#### 6. Create a MySQL Database, User, and Grant Privileges

```bash
mysql
CREATE USER '{db_user}'@'localhost' IDENTIFIED BY '{db_pwd}';
CREATE DATABASE {db_name};
GRANT ALL PRIVILEGES ON {db_name} . * TO '{db_user}'@'localhost';
```

### Other Configurations

#### 7. Run Migrations

```bash
symfony console doctrine:migrations:migrate
```

#### 8. Install Node, NPM, Yarn

- **For macOS:**
    ```bash
    brew install node
    ```

- **For Linux:**
    ```bash
    sudo apt install nodejs npm
    ```
- For better compatibility with the actual node version on our servers, it is possible to use [nvm](https://github.com/nvm-sh/nvm). All you have to do is to install nvm and run
    ```bash
    nvm use
    ```
  On project's root directory

For both systems, install Yarn:
```bash
npm i -g yarn
```

#### 9. Install Dependencies

```bash
composer install
yarn
```

#### 10. Create the First User

You can either:
* Write a custom command such as this one [AddUserCommand.php](https://github.com/symfony/demo/blob/main/src/Command/AddUserCommand.php)
* Or create it straight into the database [Generating a Password for the Admin User](https://symfony.com/doc/6.2/the-fast-track/en/15-security.html#generating-a-password-for-the-admin-user)

#### 11. Install Certificates for Local SSL

```bash
symfony server:ca:install
```

#### 12. Setup Tests

```bash
mysql
CREATE DATABASE {test_db_name}; GRANT ALL PRIVILEGES ON {test_db_name} . * TO '{db_user}'@'localhost';
symfony console doctrine:migrations:migrate --env=test
php ./vendor/bin/simple-phpunit tests
```

## Deploy to a Real Server

You can deploy to almost any server, any OS, on any web server, on an IAAS or a PAAS infrastructure, on a raw VM or on a dockerized infrastructure. The easiest way is, according to me, to use Symfony Cloud (Platform.sh): [Symfony Cloud](https://symfony.com/cloud/).

For more information about the deployment of a Symfony Application and web server configuration, please follow the official Symfony documentation:

* [Deployment](https://symfony.com/doc/current/deployment.html)
* [Web Server Configuration](https://symfony.com/doc/current/setup/web_server_configuration.html)