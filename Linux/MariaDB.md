### Install

```bash
sudo install mariadb-server

# check
sudo systemctl status mariadb
```

### Mysql secure install to protect mariaDB server

```bash
sudo mysql_secure_installation
```

### Enter mariadb CLI

```bash
sudo mariadb
```

### Create DB

```bash
sudo mariadb

CREATE DATABASE nextcloud;

GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost' IDENTIFIED BY 'mypassword';

FLUSH PRIVILEGES;
```

