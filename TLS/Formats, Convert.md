### Check if file is PEM

```bash
openssl x509 -in FILE
```

### To check if file is DER format 

```bash
openssl x509 -in FILE -inform DER
```

### To check if file is PFX format 

```bash
openssl pkcs12 -in FILE -nodes

# To check or convert PEM or DER Key Files use openssl pkey instead of openssl x509 and same command arguments
```

### Convert PEM Certificate file to DER

```bash
openssl x509 -in CERT.pem -outform DER -out CERT.der
```

### Convert DER Certificate file to PEM

```bash
openssl x509 -in CERT.der -inform der -out CERT.pem
```

### Convert PEM Certificate(s) to PFX

```bash
openssl pkcs12 -in CERTS.pem -nokeys -export -out CERTS.pfx

# To include a key in PFX file use -inkey KEY.pem instead of -nokeys
```

### To extract everything within a PFX file as a PEM file

```bash
openssl pkcs12 -in FILE.pfx -out EVERYTHING.pem -nodes
```

### To extract only the Private Key from a PFX file as PEM

```bash
openssl pkcs12 -in FILE.pfx -out KEY.pem -nodes -nocerts

# PFX files can contain Certificate(s), or Certificate(s) + one matching Key
-clcerts - extract only end-entity certificate (client certificate)
-cacerts - extract all but end-entity certificate
-nokeys - extract only certficiates
```

