# Ubuntu 22.04

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
- `sudo apt install php8.2 php8.2-fpm php8.2-mysql php8.2-opcache php8.2-curl php8.2-xml php8.2-zip unzip`
- `php -v`
- `sudo systemctl status php8.2-fpm`

**4. Install Node.js:**
- `curl -fsSL https://deb.nodesource.com/setup_22.x -o nodesource_setup.sh`
- `sudo -E bash nodesource_setup.sh`
- `sudo apt install nodejs`
- `node -v`
- `npm -v`

**5. Install Composer:**
- Docs: `https://getcomposer.org/download`
- `composer --version`

**6. Using Swap to avoid RAM overflow (Optional):**
- Docs: `https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-22-04`

**7. Set up the Laravel project:**
- Create a ssh key:
  - `ssh-keygen`
  - `cat ~/.ssh/id_rsa.pub` // copy the public key
- Set up the public key to GitHub
- `sudo chown -R "$USER":"$USER" /var/www`
- Clone the repo to the folder `/var/www`
- Cd to the project
- Checkout to the branch you need
- `composer i`
- `cp .env.example .env`
- Set up the file .env
- `php artisan key:generate`
- `php artisan migrate`
- `php artisan db:seed`
- `npm i`
- `npm run build`
- `sudo nano /etc/nginx/sites-available/{web_name}`
  
  - ```
    server {
        listen 80;
        listen [::]:80;
        server_name {web_domain} www.{web_domain};
        root /var/www/{web_repo_name}/public;
    
        add_header X-Frame-Options "SAMEORIGIN";
        add_header X-Content-Type-Options "nosniff";
    
        index index.php;
    
        charset utf-8;
    
        location / {
            try_files $uri $uri/ /index.php?$query_string;
        }
    
        location = /favicon.ico { access_log off; log_not_found off; }
        location = /robots.txt  { access_log off; log_not_found off; }
    
        error_page 404 /index.php;
    
        location ~ ^/index\.php(/|$) {
            fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
            fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
            include fastcgi_params;
            fastcgi_hide_header X-Powered-By;
        }
    
        location ~ /\.(?!well-known).* {
            deny all;
        }
    }
    ```
- `sudo ln -s /etc/nginx/sites-available/{web_name} /etc/nginx/sites-enabled/`
- `sudo nginx -t`
- `sudo systemctl reload nginx`
- `sudo chown -R www-data:www-data storage`
- `sudo chmod -R 777 storage` (optional)
