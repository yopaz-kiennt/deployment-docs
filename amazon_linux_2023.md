1. Install Nginx
- `sudo dnf update`
- `sudo dnf install -y nginx`
- `sudo systemctl start nginx.service`
- `sudo systemctl status nginx.service`
- `sudo systemctl enable nginx.service`
2. Install PHP
- `sudo dnf install php8.2 -y`
- `php -v`
- `php-fpm -v`
- `sudo dnf install php-mysqlnd`
- `sudo nano /etc/php-fpm.d/www.conf`
  - `user = nginx`
  - `group = nginx`
- `sudo systemctl start php-fpm`
- `sudo systemctl restart nginx`
3. Install MySQL
- `sudo wget https://dev.mysql.com/get/mysql80-community-release-el9-3.noarch.rpm`
- `sudo dnf install mysql80-community-release-el9-3.noarch.rpm`
- `sudo dnf update`
- `sudo dnf install mysql-community-server`
- `sudo systemctl start mysqld`
- `sudo systemctl enable mysqld`
- `sudo systemctl status mysqld`
4. Secure MySQL
- `sudo grep 'temporary password' /var/log/mysqld.log` // get temporary password
- `sudo mysql_secure_installation -p`
- `mysql -u root -p`
5. Install Node.js
- `sudo dnf update`
- `sudo curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -`
- `sudo dnf install nodejs`
- `node -v`
- `npm -v`
- `curl -fsSL https://get.pnpm.io/install.sh | sh -`
- `source /home/ec2-user/.bashrc`
- `pnpm -v`
