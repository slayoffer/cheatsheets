### Config on primary (master) server

```bash
# on primary server
sudo vim /etc/mysql/conf.d/mysql.cnf

# add blank line under [mysql] and insert
[mysqld]
log-bin
binlog-do-db=mysampledb
server-id=1

sudo vim /etc/mysql/mariadb.conf.d/50-server.cnf

# change bind-address to
bind-address = 0.0.0.0 # always restart mariadb after changing this

# enter mariadb shell and create slave user with permissions
GRANT REPLICATION SLAVE ON *.* to 'replicate'@'192.168.1.39' IDENTIFIED BY 'password'
FLUSH PRIVILEGES;

sudo systemctl restart mariadb
```

### Config on secondary (slave, backup) server

```bash
# halt data processing on primary server
FLUSH TABLES WITH READ LOCK; # will stop DB!

# backup db
mysqldump -u admin -p --databases mysampledb > mysampledb.sql

# scp db to slave server

# import db on slave server
sudo mariadb -u admin -p < mysampledb.sql

# edit config
sudo vim /etc/mysql/conf.d/mysql.cnf
# add blank line under [mysql] and insert
[mysqld]
server-id=2 # each slave server mush have unique id

sudo systemctl restart mariadb

# enter db shell
CHANGE MASTER TO MASTER_HOST="192.168.1.38",
MASTER_USER='replicate',
MASTER_PASSWORD='password';

# unlock db on master server
UNLOCK TABLES;

# check replication status on slave server
SHOW SLAVE STATUS \G; # \G for vertical text output

# should print 'Waiting for master to send event'

# if text is blank
START SLAVE;
SHOW SLAVE STATUS \G;

# to stop slave
STOP SLAVE; # for debug, resync, etc
```

### Check replication

```bash
# on master db shell
USE mysampledb;
INSERT INTO Employees VALUES ('Optimus', '100', 'Transformer');

# on slave db shell
USE mysampledb;
SELECT * FROM Employees;
```

### Debug

```bash
# check port
ss -tulpn | grep mysqld # should show 0.0.0.0 instead of 127.0.0.1
```

