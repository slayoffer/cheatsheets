### Config

```bash
# to allow all connections add an entry 
vim /etc/postgresql/15/main/pg_hba.conf

host all all 0.0.0.0/0 md5 # to the end of the file
```

### Show config file location

```bash
ps aux  | grep 'postgres *-D'

# or get into PSQL CLI
SHOW hba_file;

# or with script
psql -t -P format=unaligned -c 'show hba_file';
```

