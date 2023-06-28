### Enable SSL module

```bash
sudo a2enmod ssl

sudo systemctl restart apache2
```

### Enable SSL config

```bash
# check
sudo vim /etc/apache2/sites-available/default-ssl.conf

# enable
sudo a2ensite default-ssl.conf
```

### Create self-signed TLS certificate (OK for Dev ONLY)

```bash
sudo mkdir /etc/apache2/certs

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/certs/mysite.key -out /etc/apache2/certs/mysite.crt

sudo vim /etc/apache2/sites-available/default-ssl.conf
# comment out
SSLCertificateFile
SSLCertificateKeyFile
# insert below
SSLCertificateFile /etc/apache2/certs/mysite.crt
SSLCertificateKeyFile /etc/apache2/certs/mysite.key
# insert underneath <VirtualHost _default_:443>
ServerName mydomain.com:443

sudo systemctl reload apache2
```

### Create signed TLS

```bash
# for paid TLS need to generate CSR and then send key files to SSL dealer
openssl req -new -newkey rsa:2048 -nodes -key-out server.key -out server.csr

# for free TLS use Lets Encrypt bot
sudo apt update
sudo apt install python3-certbot-apache
sudo certbot --apache -d geekconsole.app -d www.geekconsole.app

# Only valid for 90 days, test the renewal process with
certbot renew --dry-run

sudo vim /etc/apache2/sites-available/default-ssl.conf
# comment out
SSLCertificateFile
SSLCertificateKeyFile
# insert below
SSLCertificateFile /etc/apache2/certs/mysite.crt
SSLCertificateKeyFile /etc/apache2/certs/mysite.key
# insert underneath <VirtualHost _default_:443>
ServerName mydomain.com:443

sudo systemctl reload apache2

# for several websites use default-ssl.conf as template and copy it to new config file
sudo vim /etc/apache2/sites-available/acme-consulting.conf
<IfModule mod_ssl.c> 
        <VirtualHost *:443> 
                ServerName acmeconsulting.com:443 
 
                ServerAdmin webmaster@localhost 
 
                DocumentRoot /var/www/acmeconsulting 
                ErrorLog ${APACHE_LOG_DIR}/acmeconsulting.com-error.log 
                CustomLog ${APACHE_LOG_DIR}/acmeconsulting.com-access.                  
                log combined 
 
                SSLEngine on 
        SSLCertificateFile      /etc/apache2/certs/acmeconsulting/acme.           
        crt 
        SSLCertificateKeyFile /etc/apache2/certs/acmeconsulting/acme.                
        key
```

