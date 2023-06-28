## Creating a Self-Signed Certificate

A self-signed certificate is a certificate that's signed with its own private key. It can be used to encrypt data just as well as CA-signed certificates, but our users will be shown a warning that says the certificate isn't trusted.

You can create self-signed certificates using **OpenSSL**

**OpenSSL** is a handy utility to create self-signed certificates. You can use OpenSSL on all the operating systems such as Windows, MAC, and Linux flavors.

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout domain.key -out domain.crt
```

Here, **Rivest-Shamir-Adleman (RSA)** is an asymmetric encryption algorithm and **-nodes** (short for "no DES") is used if you don't want to protect your private key with a passphrase. Otherwise it will prompt you for "at least a 4 character" password.

Decode Certificate

```
openssl x509 -in domain.crt -text -noout
```

## Creating a Self-Signed Certificate with CSR and Private key

Create a Private key

```
openssl genrsa -out domain.key 2048
```

Create CSR

```
openssl req -nodes -key domain.key -new -out domain.csr
```

We can also create both the private key and CSR with a single command:

```
openssl req -newkey rsa:2048 -nodes -keyout domain.key -out domain.csr
```

Let's create a self-signed certificate (domain.crt) with our existing private key and CSR

```
openssl x509 -signkey domain.key -in domain.csr -req -days 365 -out domain.crt
```

We can also create a private key and a self-signed certificate with a single command

```
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout domain.key -out domain.crt
```

### Generate Self-certificate with existing Private Key file 

```bash
openssl req -x509 -key KEY.pem -out CERT.pem
```

### Generate Self-certificate and new Private Key file 

```bash
openssl req -x509 -newkey <alg:opt> -nodes -out CERT.pem
```

## Links

```
https://kubernetes.io/docs/tasks/administer-cluster/certificates/#openssl
https://certlogik.com/decoder/
https://stackoverflow.com/questions/43665243/invalid-self-signed-ssl-cert-subject-alternative-name-missing
https://stackoverflow.com/questions/6194236/openssl-certificate-version-3-with-subject-alternative-name
https://serverfault.com/questions/845766/generating-a-self-signed-cert-with-openssl-that-works-in-chrome-58
Free SSL Certs: https://www.cminds.com/blog/wordpress/5-best-free-ssl-certificates-secure-site/
```