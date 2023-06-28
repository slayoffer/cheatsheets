### Status check

```bash
# slow (full) check
sudo systemctl status 'postgresql*'

# fast check
sudo systemctl list-units --type=service --state=running | grep postgresql
```

### Connect DB

```bash
psql -h localhost -p 5432 -U slayo
```

### Create admin user

```sql
CREATE USER slayo WITH SUPERUSER ENCRYPTED PASSWORD 'Sl1mthugga4life';
```

### List users

```bash
\du+;
# or
\du;
```

### List DBs

```bash
\l
# or with sizes
\l+
```

### Create DB

```sql
CREATE DATABASE slayo;
```

### Give permissions to DB

```sql
GRANT ALL PRIVILEGES ON DATABASE slayo TO slayo;
```

### Reload PG

```bash
# via CLI
psql -h localhost -U postgres -d postgres
SELECT pg_reload_conf();

# via SystemD
sudo systemctl reload postgresql

# via PG services
cd /usr/lib/postgresql/<version>/bin
./pg_ctl reload -D /var/lib/pgsql/data
# or if other PG dir
./pg_ctl reload -D /etc/postgresql/<version>/main/
```