# Ubuntu 24.04

**1. Install Nginx:**
- `sudo apt remove apache2`
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
- `sudo apt install php8.2 php8.2-fpm php8.2-mysql php8.2-opcache php8.2-zip unzip`
- `sudo systemctl status php8.2-fpm`

**4. Install Composer:**
- Docs: `https://getcomposer.org/download`
- `echo 'export PATH="$PATH:$HOME/.config/composer/vendor/bin"' >> ~/.bashrc`
- `source ~/.bashrc`
- `composer --version`

**4. Set up the Laravel project:**
- Create a ssh key:
  - `ssh-keygen`
  - `cat ~/.ssh/id_ed25519.pub` // copy the public key
- Set up the public key to GitHub
- `sudo chown -R "$USER":"$USER" /var/www`
- Clone the repo to the folder `/var/www`
- Cd to the project
- Checkout to the branch you need
