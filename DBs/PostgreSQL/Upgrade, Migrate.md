The new [PostgreSQL 15](https://www.postgresql.org/about/news/postgresql-15-released-2526/) has been released. There are multiple ways to upgrade from the old version 14, and the easiest one is by using the [pg_upgrade](https://www.postgresql.org/docs/15/pgupgrade.html) tool. Here is a quick tutorial for Ubuntu (or Debian) systems. And, please, do not forget to back up your data!

Update packages and install the new PostgreSQL 15.

```bash
sudo apt-get update
sudo apt-get install postgresql-15 postgresql-server-dev-15
```

Check if there are any differences in the config files.

```bash
diff /etc/postgresql/14/main/postgresql.conf /etc/postgresql/15/main/postgresql.conf
diff /etc/postgresql/14/main/pg_hba.conf /etc/postgresql/15/main/pg_hba.conf
```

Stop the PostgreSQL service.

```bash
sudo systemctl stop postgresql.service
```

Log in as the `postgres` user.

```bash
sudo su - postgres
```

Check clusters (notice the `--check` argument, this will not change any data).

```bash
/usr/lib/postgresql/15/bin/pg_upgrade \
  --old-datadir=/var/lib/postgresql/14/main \
  --new-datadir=/var/lib/postgresql/15/main \
  --old-bindir=/usr/lib/postgresql/14/bin \
  --new-bindir=/usr/lib/postgresql/15/bin \
  --old-options '-c config_file=/etc/postgresql/14/main/postgresql.conf' \
  --new-options '-c config_file=/etc/postgresql/15/main/postgresql.conf' \
  --check
```

Migrate the data (without the `--check` argument).

```bash
# Better do it with Screen or & !!!

screen -S pg_upgrade

/usr/lib/postgresql/15/bin/pg_upgrade \
  --old-datadir=/var/lib/postgresql/14/main \
  --new-datadir=/var/lib/postgresql/15/main \
  --old-bindir=/usr/lib/postgresql/14/bin \
  --new-bindir=/usr/lib/postgresql/15/bin \
  --old-options '-c config_file=/etc/postgresql/14/main/postgresql.conf' \
  --new-options '-c config_file=/etc/postgresql/15/main/postgresql.conf'

# Detach from the `screen` session by pressing `Ctrl` + `A`, followed by `D`. The command will continue to run in the background.

# to reattach
screen -r pg_upgrade

# or just use &
/usr/lib/postgresql/15/bin/pg_upgrade \
  --old-datadir=/var/lib/postgresql/14/main \
  --new-datadir=/var/lib/postgresql/15/main \
  --old-bindir=/usr/lib/postgresql/14/bin \
  --new-bindir=/usr/lib/postgresql/15/bin \
  --old-options '-c config_file=/etc/postgresql/14/main/postgresql.conf' \
  --new-options '-c config_file=/etc/postgresql/15/main/postgresql.conf' &

# and then check upgrade logs
sudo su
cd /var/lib/postgresql/15/main/pg_upgrade_output.d/
tail -f pg_upgrade_internal.log

```

Go back to the regular user.

```bash
exit
```

Swap the ports for the old and new PostgreSQL versions.

```bash
sudo vim /etc/postgresql/15/main/postgresql.conf
# ...and change "port = 5433" to "port = 5432"

sudo vim /etc/postgresql/14/main/postgresql.conf
# ...and change "port = 5432" to "port = 5433"
```

Start the PostgreSQL service.

```bash
sudo systemctl start postgresql.service
```

Log in as the `postgres` user again.

```bash
sudo su - postgres
```

Check the new PostgreSQL version.

```bash
psql -c "SELECT version();"
```

Run the recommended `vacuumdb` command:

```bash
/usr/lib/postgresql/15/bin/vacuumdb --all --analyze-in-stages
```

Back to normal user.

```bash
exit
```

Check which old PostgreSQL packages are installed.

```bash
apt list --installed | grep postgresql
```

Remove the old PostgreSQL packages (from the listing above).

```bash
sudo apt-get remove postgresql-14 postgresql-server-dev-14
```

Remove the old configuration.

```bash
sudo rm -rf /etc/postgresql/14/
```

Log in as the `postgres` user once more.

```bash
sudo su - postgres
```

Finally, drop the old cluster data.

```bash
./delete_old_cluster.sh
```

Done!