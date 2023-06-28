## SSL Certificates For PostgreSQL

This describes how to set up ssl certificates to enable encrypted connections from PgAdmin on some client machine to postgresql on a server machine. The assumption is that postgresql (compiled with ssl support) and openssl are already installed and functional on the server (Linux). PgAdmin is already installed on the client (either Windows or Linux).

On the server, three certificates are required in the data directory. CentOS default is */var/lib/pgsql/data/*:
*root.crt* (trusted root certificate)
*server.crt* (server certificate)
*server.key* (private key)

Issue commands as root.

```
sudo -
cd /var/lib/pgsql/data
```

Generate a private key (you must provide a passphrase).

```
openssl genrsa -des3 -out server.key 1024
```

Remove the passphrase.

```
openssl rsa -in server.key -out server.key
```

Set appropriate permission and owner on the private key file.

```
chmod 400 server.key
chown postgres.postgres server.key
```

Create the server certificate.
-subj is a shortcut to avoid prompting for the info.
-x509 produces a self signed certificate rather than a certificate request.

```
openssl req -new -key server.key -days 3650 -out server.crt -x509 -subj '/C=CA/ST=British Columbia/L=Comox/O=TheBrain.ca/CN=thebrain.ca/emailAddress=info@thebrain.ca'
```

Since we are self-signing, we use the server certificate as the trusted root certificate.

```
cp server.crt root.crt
```

You'll need to edit *pg_hba.conf*. For example:

```
# TYPE  DATABASE    USER        CIDR-ADDRESS          METHOD
# "local" is for Unix domain socket connections only
local   all         all                               trust
# IPv4 local connections:
host    all         all         127.0.0.1/32          trust# IPv4 remote connections for authenticated users
hostssl all         www-data    0.0.0.0/0             md5 clientcert=1hostssl all         postgres    0.0.0.0/0             md5 clientcert=1
```

You need to edit *postgresql.conf* to actually activate ssl:

```
ssl = on
```

Postgresql server must be restarted.

```
/etc/init.d/postgresql restart
```

If the server fails to (re)start, look in the postgresql startup log, */var/lib/pgsql/pgstartup.log* default for CentOS, for the reason.

On the client, we need three files. For Windows, these files must be in *%appdata%\postgresql\* directory. For Linux *~/.postgresql/* directory.
*root.crt* (trusted root certificate)
*postgresql.crt* (client certificate)
*postgresql.key* (private key)

Generate the the needed files on the server machine, and then copy them to the client. We'll generate the needed files in the */tmp/* directory.

First create the private key *postgresql.key* for the client machine, and remove the passphrase.

```
openssl genrsa -des3 -out /tmp/postgresql.key 1024
openssl rsa -in /tmp/postgresql.key -out /tmp/postgresql.key
```

Then create the certificate *postgresql.crt*. It must be signed by our trusted root (which is using the private key file on the server machine). Also, **the certificate \*common name\* (CN) must be set to the database user name we'll connect as**.

```
openssl req -new -key /tmp/postgresql.key -out /tmp/postgresql.csr -subj '/C=CA/ST=British Columbia/L=Comox/O=TheBrain.ca/CN=www-data'
openssl x509 -req -in /tmp/postgresql.csr -CA root.crt -CAkey server.key -out /tmp/postgresql.crt -CAcreateserial
```

Copy the three files we created from the server */tmp/* directory to the client machine.

Copy the trusted root certificate *root.crt* from the server machine to the client machine (for Windows pgadmin *%appdata%\postgresql\* or for Linux pgadmin *~/.postgresql/*). Change the file permission of *postgresql.key* to restrict access to just you (probably not needed on Windows as the restricted access is already inherited). Remove the files from the server */tmp/* directory.