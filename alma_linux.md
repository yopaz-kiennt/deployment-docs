# AlmaLinux 8.10 

**1. Log slow queries:**
- `sudo nano /etc/my.cnf`

  - ```
    [mysqld]
    slow_query_log = 1
    slow_query_log_file = /var/log/mysql/slow.log
    long_query_time = 0.3
    ```
- `sudo systemctl restart mariadb`
- `sudo dnf install https://repo.percona.com/yum/percona-release-latest.noarch.rpm`
- `sudo percona-release enable tools release`
- `sudo dnf install -y percona-toolkit`
- `pt-query-digest --version`
- `pt-query-digest /var/log/mysql/slow.log`
