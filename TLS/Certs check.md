### Check server cert

```bash
openssl s_client --connect reddit.com:443

# copy cert to some file and use cmd
openssl x509 -in cert.file --text --noout # --noout to dont show bases64 content of cert

# to match private key with cert file compare their modulus sections
openssl rsa -in yellow.key --modulus --noout
openssl x509 -in yellow.cert --modulus --noout
```

### Cmds

```bash
# The openssl x509 utility also allows you to extract specific pieces of information from the certificate file instead of the entire content of a certificate in text

# Use openssl x509 utility to request specific fields from a Certificate:
openssl x509 -in google.com-cert -noout -serial
openssl x509 -in google.com-cert -noout -issuer
openssl x509 -in google.com-cert -noout -dates
openssl x509 -in google.com-cert -noout -subject
openssl x509 -in google.com-cert -noout -pubkey
openssl x509 -in google.com-cert -noout -modulus
openssl x509 -in google.com-cert -noout -ocsp_uri
# Note: Last command may not work in all versions of OpenSSL

# You can also mix and match arguments from the last step:
openssl x509 -in google.com-cert -noout -subject -issuer
openssl x509 -in google.com-cert -noout -serial -dates

# You can also request specific extensions from the certificate:
openssl x509 -in google.com-cert -noout -ext subjectAltName
openssl x509 -in google.com-cert -noout -ext basicConstraints
openssl x509 -in google.com-cert -noout -ext crlDistributionPoints
openssl x509 -in google.com-cert -noout -ext keyUsage
openssl x509 -in google.com-cert -noout -ext extendedKeyUsage
openssl x509 -in google.com-cert -noout -ext authorityInfoAccess
openssl x509 -in google.com-cert -noout -ext subjectKeyIdentifier
openssl x509 -in google.com-cert -noout -ext authorityKeyIdentifier
# Note: Requesting specific extensions may not work in all versions of OpenSSL

# You also have the ability to request multiple extensions by separating them with a comma:
openssl x509 -in google.com-cert -noout -ext keyUsage,extendedKeyUsage
... -ext subjectKeyIdentifier,authorityKeyIdentifier
... -ext crlDistributionPoints,authorityInfoAccess
```



### Viewing x509 Certificate as human readable Text 

```bash
openssl x509 -in CERT.pem -noout -text
```

### Viewing Certificate Signing Request (CSR) contents as Text 

```bash
openssl req -in CSR.pem -noout -text
```

### Extract specific pieces of information from x509 Certificates 

```bash
penssl x509 -in CERT.pem -noout -dates
openssl x509 -in CERT.pem -noout -issuer –subject

# Other items you can extract:
-modulus -pubkey -ocsp_uri -ocspid
-serial -startdate -enddate
```

### Extract specific Extension(s) from a certificate 

```bash
openssl x509 -in CERT.pem -noout -ext subjectAltName
openssl x509 -in CERT.pem -noout -ext authorityInfoAccess,crlDistributionPoints

# Other extensions you can extract: 
basicConstraints nameConstraints certificatePolicies
keyUsage extendedKeyUsage subjectKeyIdentifier authorityKeyIdentifier
```

### Extract all Extensions from a certificate

```bash
openssl x509 -in CERT.pem -noout -text | sed '/X509v3 extensions/,/Signature Algorithm:/!d'
```
