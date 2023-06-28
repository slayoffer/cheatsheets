### Converting any Private Key file into text (RSA, DSA, or EC) 

```bash
openssl pkey -in KEY.pem -noout -text
```

### Extracting only Public Key as text from any Key file  

```bash
openssl pkey -in KEY.pem -noout -text_pub
```

### Extracting only Public Key in PEM format 

```bash
openssl pkey -in KEY.pem -pubout

# pkey expects a Private Key file. Public Key file can be read with -pubin
```

### List all cipher algorithms for encryption

```bash
openssl list -cipher-algorithms
```

### Check server cert

```bash
openssl s_client --connect reddit.com:443
```

### Formats

```bash
DER - binary encoded # not used for exchaning files
# Uses .der, .cert, .crt, .csr so need to check whether its DER or not
# Mostly used for: Java

PEM - Base64 encoded # most widely used, can be catt'ed, can contain private, public and csr sections inside
# Uses .pem, .key, .cert, .crt, .csr so need to check whether its PEM or not
# Mostly used for: Nginx, Apache, Linux, Plesk, Cisco, BigIP

PFX/PKCS12 - same crypto standart nowadays # PFX is very rare, PKCS is widely used, can contain cert chains
# binary, so cant be catt'ed, uses .pfx, .p12, .pkcs12
# Mostly used for: IIS, Windows, Tomcat, Thunderbird, Firefox

PKCS7 - Base64 encoded # only contains certs and chains, no keys
# mostly used for mail, uses .pkcs7, .p7b, .p7c
# Mostly used for: S/MIME, java, Windows
```

