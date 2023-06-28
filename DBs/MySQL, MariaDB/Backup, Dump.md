### Dump

When using any particular method to clone disks on a live Linux environment, your only concern would likely be with the databases. The best way to backup and restore a database is to use their dump tool to make an ascii file snapshot of the database just prior to the file system dump. For mysql there is :

```bash
mysqldump --all-databases > mysql_databases.sql
```

### Backup

```bash
# to backup
mysqldump -u admin -p --databases mysampledb > mysampledb.sql

# to restore
sudo mariadb -u admin -p < mysampledb.sql
```
