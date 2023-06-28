### Settings

```bash
cat /etc/system-release ; hostname ; hostname -I ; sestatus ; dnf groupinstall "Development Tools" -y  
sudo dnf install https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm  
dnf -qy module disable postgresql ; dnf -y install postgresql14 postgresql14-server  
/usr/pgsql-14/bin/postgresql-14-setup initdb  
systemctl start postgresql-14 ; systemctl enable postgresql-14 ; systemctl status postgresql-14  
  
# Set PostgreSQL Admin Password -  
su - postgres  
psql -c "alter user postgres with password 'yourpassword'"  
exit  
  
# Self-Signed SSL Certificate -  
cd /etc/pki/tls/certs ;  openssl genrsa -aes128 2048 > server.key ; openssl rsa -in server.key -out server.key    | root  
openssl req -utf8 -new -key server.key -out server.csr  
openssl x509 -in server.csr -out server.crt -req -signkey server.key -days 3650  
chmod 600 server.key  
cp /etc/pki/tls/certs/server.{crt,key} /var/lib/pgsql/14/data/  
chown postgres. /var/lib/pgsql/14/data/server.{crt,key} ; chmod 600 /var/lib/pgsql/14/data/server.{crt,key}  
  
nano /var/lib/pgsql/14/data/postgresql.conf

listen_addresses = '*'  
ssl = on  
ssl_cert_file = 'server.crt'  
ssl_key_file = 'server.key'  

nano /var/lib/pgsql/14/data/pg_hba.conf

host all all 127.0.0.1/32 ident - host all all 0.0.0.0/0 md5  
hostssl all             all             0.0.0.0/0             md5  
  
systemctl restart postgresql-14 ; systemctl status postgresql-14  
firewall-cmd --add-service=postgresql --permanent ; firewall-cmd --reload  
________________________________________________________________________________________  

cat /etc/system-release ; hostname ; hostname -I ; sestatus ; dnf groupinstall "Development Tools" -y  
dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-8-x86_64/pgdg-redhat-repo-latest.noarch.rpm  
dnf -qy module disable postgresql ; dnf install -y postgresql14-server  
/usr/pgsql-14/bin/postgresql-14-setup initdb  
systemctl start postgresql-14 ; systemctl enable postgresql-14 ; firewall-cmd --add-service=postgresql --permanent ; firewall-cmd --reload  
sestatus ; hostname -I  
su - postgres  
psql -U postgres -p 5432 -h 192.168.1.60           |  yourpassword  

_________________________________________________________________________________________  
```