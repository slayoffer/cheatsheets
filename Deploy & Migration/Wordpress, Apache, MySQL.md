### Ubuntu

```bash
apt update && apt upgrade

apt install apache2 wget -y

systemctl status apache2 # check https://ip-address

apt install mariadb-server mariadb-client

# secure our MariaDB database engine and disallow remote root login
mysql_secure_installation

apt install php php-mysql

vim /var/www/html/info.php

# append the following lines:
<?php
phpinfo();
?>

# check https://ip-address/info.php

# create WordPress Database
mysql -u root -p

CREATE DATABASE wordpress_db;
CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'password';
GRANT ALL ON wordpress_db.* TO 'wp_user'@'localhost' IDENTIFIED BY 'password';
FLUSH PRIVILEGES;
Exit;

cd /tmp

wget https://wordpress.org/latest.tar.gz

tar -xvf latest.tar.gz

cp -R wordpress /var/www/html/

chown -R www-data:www-data /var/www/html/wordpress/

chmod -R 755 /var/www/html/wordpress/

mkdir /var/www/html/wordpress/wp-content/uploads

chown -R www-data:www-data /var/www/html/wordpress/wp-content/uploads/

# check https://server-ip/wordpress
```

