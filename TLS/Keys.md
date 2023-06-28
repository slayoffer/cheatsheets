### Read key

```bash
# to just cat the key
openssl rsa -in yellow.key

# for text version
openssl rsa -in yellow.key --text --noout

# to match private key with cert file compare their modulus sections
openssl rsa -in yellow.key --modulus --noout
openssl x509 -in yellow.cert --modulus --noout
```

