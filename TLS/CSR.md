## Create Private Key and CSR

Create the Server Private Key

```
openssl genrsa -out domain.key 2048
```

Now, we have a key, we need a certificate signing request (CSR). Lets start with a `csr.conf`(CSR Configuration) with the below contents

```
cat > csr.conf <<EOF
[ req ]
default_bits = 2048
prompt = no
default_md = sha256
req_extensions = req_ext
distinguished_name = req_distinguished_name

[req_distinguished_name]
C = IN
ST = Karnataka
L = Bengaluru
O = DME
OU = DevSecOps
CN = test.mynginx.com

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = test.mynginx.com
DNS.2 = mynginx.com
DNS.3 = www.mynginx.com 
IP.1 = 13.233.110.41

EOF
```

Generate CSR using Private Key of the domain

```
openssl req -new -key domain.key -out domain.csr -config csr.conf
```

Decode CSR and observe the SAN field. It will be present in CSR

```
openssl req -text -in domain.csr
```

Sign CSR with CA and Genereate SSL certificate

```
openssl x509 -req \
    -in domain.csr \
    -CA rootCA.crt -CAkey rootCA.key \
    -CAcreateserial -out domain.crt \
    -days 365 
```

Decode Certificate and observe that SAN field is missing from certificate while it is available in CSR. The reason is that by default OpenSSL does not copy extensions from the request to the certificate. We can fix this by providing the extensions file

```
openssl x509 -in domain.crt -text -noout
```

Create a external file to include SAN in the certificate

```
cat > v3.ext <<EOF

authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = test.mynginx.com
DNS.2 = mynginx.com
DNS.3 = www.mynginx.com 
IP.1 = 13.233.110.41

EOF
```

Generate SSL certificate With self signed CA

```
openssl x509 -req \
    -in domain.csr \
    -CA rootCA.crt -CAkey rootCA.key \
    -CAcreateserial -out domain.crt \
    -days 365 \
    -extfile v3.ext
```

Decode Certificate and observe that SAN is available in the certificate

```
openssl x509 -in domain.crt -text -noout
```

we can also include extensions directly in the `csr.conf`

```
cat > csr.conf <<EOF
[ req ]
default_bits = 2048
prompt = no
default_md = sha256
req_extensions = req_ext
distinguished_name = dn

[ dn ]
C = IN
ST = Karnataka
L = Bangalore
O = DME
OU = DevSecOps
CN = mynginx.com

[ req_ext ]
subjectAltName = @alt_names

[ alt_names ]
DNS.1 = mynginx.com
DNS.2 = test.mynginx.com
DNS.3 = www.mynginx.com 
IP.1 = 13.233.110.41

[ v3_ext ]
authorityKeyIdentifier=keyid,issuer:always
basicConstraints=CA:FALSE
keyUsage=keyEncipherment,dataEncipherment
extendedKeyUsage=serverAuth,clientAuth
subjectAltName=@alt_names

EOF
```

Generate CSR

```
openssl req -new -key domain.key -out domain.csr -config csr.conf
```

Generate the server certificate using the ca.key, ca.crt and domain.csr

```
openssl x509 -req \
       -in domain.csr \
       -CA rootCA.crt -CAkey rootCA.key \
       -CAcreateserial -out domain.crt \
       -days 10000 \
       -extensions v3_ext -extfile csr.conf
```

Decode Certificate and observe that SAN is available in the certificate

```
openssl x509 -in domain.crt -text -noout
```

## Install and configure Nginx

Install Nginx on AWS EC2 running Linux AMI

```
yum install nginx -y
systemctl enable --now nginx
```

Enable SSL by editing the configuration `/etc/nginx/nginx.conf` to listen on 443

```
server {

listen   443;

ssl    on;
ssl_certificate    /etc/ssl/server.crt;
ssl_certificate_key    /etc/ssl/server.key;

server_name your.domain.com;
access_log /var/log/nginx/nginx.vhost.access.log;
error_log /var/log/nginx/nginx.vhost.error.log;
location / {
root   /home/www/public_html/your.domain.com/public/;
index  index.html;
}
}
```

Copy certificate and key we created to the location where nginx use in it's `/etc/nginx/nginx.conf`

```
mkdir -p /etc/pki/nginx/private
cp domain.crt /etc/pki/nginx/server.crt
cp domain.key /etc/pki/nginx/private/server.key
```

Restart Nginx

```
systemctl restart nginx
```

## Adding CA Cert in browser

```
https://support.securly.com/hc/en-us/articles/206081828-How-do-I-manually-install-the-Securly-SSL-certificate-in-Chrome-
```

### Generating CSRs 

```bash
openssl req -new -key KEY.pem -out CSR.pem
```

### Generate CSR and new Private Key file  

```bash
openssl req -new -newkey <alg:opt> -nodes -out CSR.pem
```

### Notes

```bash
Commands above will prompt you for the Subject Distinguished Name (DN) attributes. Alternatively, you can specify them using -subj:

Examples: -subj "/CN=website.com" --or-- -subj "/C=US/ST=Colorado/L=Denver/O=ACME Inc./CN=acme.com"

-nodes - Generate Key File with No DES encryption - Skips prompt for PEM Pass phrase

-<digest> - Sign CSR/Cert using <digest> hashing algorithm. View supported algorithms: openssl list --digest-commands

-config - Specify config file with custom options. Default Config file: openssl.cnf in directory specified by openssl version -d

The argument -newkey <alg:opt> lets you create RSA, DSA, or EC Keys:

-newkey 1024 - Generate 1024 bit RSA Keys (legacy) -newkey dsa:DSA-PARAM.pem - Generate DSA Keys using DSA Parameters

-newkey rsa:2048 - Generate 2048 bit RSA Keys -newkey ec:EC-PARAM.pem - Generate EC Keys using EC Parameters

If -key or -newkey is not specified, a private key file will be automatically generated using directives specified in openssl.cnf
```

