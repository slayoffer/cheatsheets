### Show settings

```bash
mysqld --help --verbose
# or
mysql --help | grep /my.cnf | xargs ls

# also might be here in Ubuntu
cat /etc/mysql/mysql.conf.d/mysqld.cnf

# find
find / -name my.cnf

# can check these for configs:
- /etc/my.cnf
- /etc/mysql/my.cnf
- $MYSQL_HOME/my.cnf
- [datadir]/my.cnf
- ~/.my.cnf
```

### Env

```bash
echo $MYSQL_HOME
```