### Generate 2048 bit RSA Private Key saved as KEY1.pem  

```bash
openssl genrsa -out KEY1.pem 2048
```

### Generate 4096 bit RSA Private Key, encrypted with AES128  

```bash
openssl genrsa -out KEY2.pem -aes128 4096

- Key size must be last argument of command
- Omit -out <FILE> argument to output to StdOut
- Other encryption algorithms are also supported:
aes128, -aes192, -aes256, -des3, -des
```

### Converting an RSA Private Key into text  

```bash
openssl rsa -in KEY.pem -noout -text
```

### Removing encryption from an RSA key file 

```bash
openssl rsa -in ENCRYPTED-KEY.pem -out KEY.pem
```

### Encrypting an RSA Key File 

```bash
openssl rsa -in KEY.pem -aes128 -out ENCRYPTED-KEY.pem
```

### Check if RSA Key matches a CSR or Cert 

```bash
# Compare Modulus values to see if files match each other
openssl req -in CSR.pem -noout -modulus
openssl x509 -in CERT.pem -noout -modulus
openssl rsa -in KEY.pem -noout -modulus
```

