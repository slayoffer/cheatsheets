To completely remove PostgreSQL 15 from an Ubuntu server, follow these steps. Please note that this process will delete all databases and their data, so ensure that you have proper backups if necessary.

Stop the PostgreSQL service:

```bash
# check running PG services and stop the needed ones
systemctl list-units --type=service --state=running | grep postgres

# or more detailed
sudo systemctl status 'postgresql*'
```

`sudo systemctl stop postgresql`

Remove the PostgreSQL 15 package and its dependencies:

Check which old PostgreSQL packages are installed.

```bash
apt list --installed | grep postgresql
# or better
dpkg -l | grep postgres

# remove the needed packages, better not use apt if need to remove specific version of PG
sudo dpkg -r --force-depends postgresql-client-15

# or purge, but be careful
sudo dpkg --purge --force-depends postgresql-15

```

Remove any PostgreSQL configuration files and data directories:

```bash
sudo rm -rf /etc/postgresql/15

sudo rm -rf /var/lib/postgresql/15

sudo rm -rf /var/log/postgresql/15
```

Remove the PostgreSQL user and group:

`sudo deluser postgres sudo delgroup postgres`

Finally, update the package list to ensure all references to PostgreSQL 15 are removed:

`sudo apt update`

This process should completely remove PostgreSQL 15 and its associated data from your Ubuntu server.