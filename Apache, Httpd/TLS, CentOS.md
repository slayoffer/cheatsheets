### Install

```bash
sudo yum install openssl
```

### Add SSL certs to config

```bash
sudo vim /etc/httpd/conf.d/ssl.conf

# add certificate to
SSLCertificateFile /etc/httpd/certs/app01.crt

# add key to
SSLCertificateKeyFile /etc/httpd/certs/app01.key

# restart apache
sudo systemctl restart httpd

# check
echo | openssl s_client -showcerts -servername app01.com -connect app01:443 2>/dev/null | openssl x509 -inform pem
```

### Request CSR

```bash
# get into webserver
cd /etc/httpd/csr

# make csr
sudo openssl req -new -newkey rsa:2048 -nodes -keyout app01.key -out app01.csr

# verify requests
openssl req -noout -text -in app01.csr
```

### Create Self Signed Certificate

```bash
cd /etc/httpd/certs

sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout app01.key -out app01.crt
```

