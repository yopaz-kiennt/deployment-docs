# Amazon Linux 2023

**1. Install Nginx:**
- `sudo dnf update`
- `sudo dnf install -y nginx`
- `sudo systemctl start nginx.service`
- `sudo systemctl status nginx.service`
- `sudo systemctl enable nginx.service`
  
**2. Install PHP:**
- `sudo dnf install php8.2 -y`
- `php -v`
- `php-fpm -v`
- `sudo dnf install php-mysqlnd`
- `sudo nano /etc/php-fpm.d/www.conf`
  - `user = nginx`
  - `group = nginx`
- `sudo systemctl start php-fpm`
- `sudo systemctl restart nginx`

**3. Install MySQL:**
- `sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-3.noarch.rpm`
- `sudo dnf install mysql80-community-release-el9-3.noarch.rpm`
- `sudo dnf update`
- `sudo dnf install mysql-community-server`
- `sudo systemctl start mysqld`
- `sudo systemctl enable mysqld`
- `sudo systemctl status mysqld`
- `sudo grep 'temporary password' /var/log/mysqld.log` // get temporary password
- `sudo mysql_secure_installation -p`
- `mysql -u root -p`

**4. Install Node.js:**
- `sudo dnf update`
- `sudo curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -`
- `sudo dnf install nodejs`
- `node -v`
- `npm -v`
- `curl -fsSL https://get.pnpm.io/install.sh | sh -`
- `source /home/"$USER"/.bashrc`
- `pnpm -v`

**5. Install Git:**
- `sudo dnf install git`

**6. Install Composer:**
- `sudo dnf update`
- `curl -sS https://getcomposer.org/installer | php`
- `sudo mv composer.phar /usr/local/bin/composer`
- `composer --version`

**7. Using Swap to avoid RAM overflow (Optional):**
- Docs: `https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-centos-7`

**8. Set up the Laravel project:**
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
- `npm i` OR `pnpm i`
- `npm run build` OR `pnpm run build`
- `cd /etc/nginx/conf.d`
- `sudo nano {web_name}.conf`
  
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
            fastcgi_pass unix:/var/run/php-fpm/www.sock;
            fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
            include fastcgi_params;
            fastcgi_hide_header X-Powered-By;
        }
    
        location ~ /\.(?!well-known).* {
            deny all;
        }
    }
    ```
- `sudo systemctl restart nginx`
- Cd to the project
- `sudo chown -R nginx:nginx storage`
- `sudo chmod -R 777 storage` (optional)

**9. Install Certbot (HTTPS):**
- `sudo dnf install -y certbot python3-certbot-nginx`
- `sudo systemctl daemon-reload`
- `sudo systemctl enable --now certbot-renew.timer`
- `sudo certbot --nginx -d {web_domain}`

**10. Install Supervisor (Queue):**
- `sudo dnf install python3 python3-pip`
- `sudo pip install supervisor`
- `sudo mkdir /etc/supervisor`
- `echo_supervisord_conf | sudo tee /etc/supervisor/supervisord.conf`
- `cd /etc/supervisor`
- `sudo mkdir conf.d`
- `cd conf.d`
- `sudo nano {web_repo_name}-worker.conf`

  - ```
    [program:{web_repo_name}-worker]
    process_name=%(program_name)s_%(process_num)02d
    command=php /var/www/{web_repo_name}/artisan queue:listen
    autostart=true
    autorestart=true
    stopasgroup=true
    killasgroup=true
    user=nginx
    numprocs=2
    redirect_stderr=true
    stdout_logfile=/var/www/{web_repo_name}/storage/logs/worker.log
    stopwaitsecs=3600
    ```
- `sudo nano supervisord.conf`

  - ```
    [include]
    files = /etc/supervisor/conf.d/*.conf
    ```
- `sudo supervisord -c /etc/supervisor/supervisord.conf`
- `sudo supervisorctl reread`
- `sudo supervisorctl update`
- `sudo supervisorctl start "{web_repo_name}-worker:*"`
