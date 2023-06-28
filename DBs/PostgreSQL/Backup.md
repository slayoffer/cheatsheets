### Dump and restore

```bash
# создание дампа в 8 потоков, в формате директории
pg_dump postgresql://$PSQL_USER:$PSQL_PASSWORD@$PSQL_HOST:$PSQL_PORT/$PSQL_DBNAME -j 8 -Fd -f /tmp/dump.dir # -F, --format=c|d|t|p output file format (custom, directory, tar, plain text (default))

# Восстановим данные из дампа, не забыв что подключиться надо к новой базе:
pg_restore -d "postgresql://$PSQL_USER:$PSQL_PASSWORD@$PSQL_HOST:$PSQL_PORT/$PSQL_DBNAME" --format=d /tmp/dump.dir/ # это выгрузка дампа в формате директории

# or
pg_dumpall > pg_databases.sql
# Then copy the backup to your development server, restore with:
psql the_new_dev_db < pg_databases.sql

# or for different Postgres versions
pg_dumpall -p 5432 -U myuser91 | psql -U myuser94 -d postgres -p 5434

# or run this command with database name, you want to backup, to take dump of DB.
pg_dump -U postgres mydbname -f mydbnamedump.sql

# Now scp this dump file to remote machine where you want to copy DB.
scp mydbnamedump.sql user01@remotemachineip:~/some/folder/

# On remote machine run following command in ~/some/folder to restore the DB.
psql -U postgres -d mynewdb -f mydbnamedump.sql
```

### Parameters

```bash
# Use pg_dump, and later psql or pg_restore - depending whether you choose -Fp or -Fc options to pg_dump.

# Example of usage:
ssh production
pg_dump -C -Fp -f dump.sql -U postgres some_database_name
scp dump.sql development:
rm dump.sql
ssh development
psql -U postgres -f dump.sql

# -F, --format=c|d|t|p output file format (custom, directory, tar, plain text (default))
```

### pg_basebackup

```bash
# This solution backs up the entire database cluster (users, databases, etc.).

# I'm posting this as a solution on here because it details every step I had to take, feel free to add recommendations or improvements after reading other answers on here and doing some more research.

# For Postgres 12 and Ubuntu 18.04 I had to do these actions:

# On the server that is currently running the database:
# Backup pg_hba.conf and postgresql.conf files and copy/move them to slave AFTER everything is done - NOT NOW!
# Update pg_hba.conf, for me located at /etc/postgresql/12/main/pg_hba.conf
# Add the following line (substitute 192.168.0.100 with the IP address of the server you want to copy the database to).
host  replication  postgres  192.168.0.100/32  trust

# Update postgresql.conf, for me located at /etc/postgresql/12/main/postgresql.conf. Add the following line:
listen_addresses = '*'

# Restart postgres:
sudo service postgresql restart

# On the host you want to copy the database cluster to:
sudo service postgresql stop
sudo su root
rm -rf /var/lib/postgresql/12/main/*
exit

sudo -u postgres pg_basebackup -h 192.168.0.101 -U postgres -D /var/lib/postgresql/12/main/
sudo service postgresql start

# Big picture - stop the service, delete everything in the data directory (mine is in /var/lib/postgreql/12). The permissions on this directory are drwx------ with user and group postgres. I could only do this as root, not even with sudo -u postgres. I'm unsure why. Ensure you are doing this on the new server you want to copy the database to! You are deleting the entire database cluster.

# Make sure to change the IP address from 192.168.0.101 to the IP address you are copying the database from. Copy the data from the original server with pg_basebackup. Start the service.

# Update pg_hba.conf and postgresql.conf to match the original server configuration - which were BEFORE you made any changes adding the replication line and the listen_addresses line (in my care I had to add the ability to log-in locally via md5 to pg_hba.conf).

# Note there are considerations for max_wal_senders and wal_level that can be found in the documentation. I did not have to do anything with this.
```

