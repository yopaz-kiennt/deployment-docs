# Ubuntu 24.04

**1. Install Nginx:**
- `sudo apt update`
- `sudo apt install nginx`
- `sudo systemctl status nginx`

**2. Install MySQL:**
- `sudo apt install mysql-server`
- `sudo systemctl status mysql`
- `sudo mysql_secure_installation`
- `sudo mysql`
- mysql> `ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';`
- mysql> `FLUSH PRIVILEGES;`
- mysql> `exit`
- `mysql -u root -p`

**3. Install PHP:**
- `sudo add-apt-repository ppa:ondrej/php`
- `sudo apt update`
- `sudo apt install php8.2-fpm`
- `sudo systemctl status php8.2-fpm`

**4. Install Composer:**
- ```
  php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
  php -r "if (hash_file('sha384', 'composer-setup.php') === 'dac665fdc30fdd8ec78b38b9800061b4150413ff2e3b6f88543c636f7cd84f6db9189d43a81e5503cda447da73c7e5b6') { echo 'Installer verified'.PHP_EOL; } else { echo 'Installer corrupt'.PHP_EOL; unlink('composer-setup.php'); exit(1); }"
  php composer-setup.php
  php -r "unlink('composer-setup.php');"
  ```
- `sudo mv composer.phar /usr/local/bin/composer`
- `composer --version`

**4. Set up the Laravel project:**
- Create a ssh key:
  - `ssh-keygen`
  - `cat ~/.ssh/id_ed25519.pub` // copy the public key
- Set up the public key to GitHub
