## Create Certificate Authority

Let's create a private key (rootCA.key) and a self-signed root CA certificate (rootCA.crt). This will act like our private CA

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout rootCA.key -out rootCA.crt
```

or you can directly provide the CSR details like Country,State, Common Name etc in the command

```
openssl req -x509 \
            -sha256 \
            -days 356 \
            -nodes \
            -newkey rsa:2048 \
            -subj "/C=IN/ST=KA/L=BLR/O=DME/OU=DevSecOps/CN=www.devopsmadeeasy.in" \
            -keyout rootCA.key -out rootCA.crt 
```

Decode Certificate

```
openssl x509 -in rootCA.crt -text -noout
```