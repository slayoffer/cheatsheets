```bash
##### Database administrative login by Unix domain socket
local   all             postgres                                peer

##### TYPE  DATABASE        USER            ADDRESS                 METHOD

##### "local" is for Unix domain socket connections only
local   all             all                                     peer
##### IPv4 local connections:
host    all             all             109.194.34.184/32       md5
host    all             all             10.128.0.0/24           md5
host    all             all             127.0.0.1/32            md5
host    all             all             127.0.1.1/32            md5
host    all             all             172.18.0.0/16           md5
##### IPv6 local connections:
# a MUST for local psql logins, backup scripts, etc !!!
host    all             all             ::1/128                 md5
##### Allow replication connections from localhost, by a user with the replication privilege.
local   replication     all                                     peer
host    replication     all             127.0.0.1/32            scram-sha-256
host    replication     all             ::1/128                 scram-sha-256
```