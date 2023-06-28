### Install MySQL

```bash
# for MySQL community server
sudo yum install https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
# optional
rpm -ivh mysql57-community-release-el7-9.noarch.rpm

sudo yum install mysql-community-server

sudo systemctl enable mysqld --now

sudo systemctl status mysqld

# default install dir
ls /var/lib/mysql
```

### Install MariaDB

```bash
# for Maria-DB server
sudo yum install mariadb-server

sudo vi /etc/my.cnf

sudo systemctl enable mariadb --now

sudo systemctl status mariadb


```

### Firewall

```bash
# CentOS
sudo firewall-cmd --permanent --zone=public --add-port=3306/tcp
sudo firewall-cmd --reload
```

### Start db

```bash
sudo systemctl start mysqld

# for Maria
sudo systemctl start mariadb
```

### Connect to db

```bash
# check default root password
sudo grep 'temporary password' /var/log/mysqld.log

# log in
mysql -u root -p # enter default password

# for Maria-DB
mysql -u root
```

### Change default password

```bash
# for MySQL
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass4!';
FLUSH PRIVILEGES;
# or
SET PASSWORD = PASSWORD('P@ssw0rd123');
FLUSH PRIVILEGES;

# for Maria-DB
mysql -u root
USE mysql;
UPDATE user SET password=PASSWORD('P@ssw0rd123') WHERE User='root' AND Host = 'localhost';
FLUSH PRIVILEGES;
```

### Create DB

```sql
CREATE DATABASE school;

SHOW DATABASES;
```

### Switch to DB

```sql
USE school;
```

### Load SQL script

```bash
sudo mysql < /opt/db-load-script.sql
```

### Logs

```bash
cat /var/log/mysqld.log # can check temp. password here
```

### Port

```bash
# default port is 3306
```

### Slow SQL responses folder

```bash
cat /var/log/mysql/mysql-slow.log

# to enable
vim /etc/mysql/my.cnf

# edit
slow_query_log          = /var/log/mysql/mysql-slow.log
long_query_time         = 1
```

