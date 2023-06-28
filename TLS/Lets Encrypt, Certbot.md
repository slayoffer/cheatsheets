### Install

```bash
sudo snap certbot --classic # classic for privileged snap install
```

### Create certs

```bash
# for manual cert creating
sudo certbot certonly --standalone
```



### Renew certs

```bash
# renews all certificates on your server
sudo certbot renew

# If you have multiple certificates for different domains and you want to renew a specific certificate, use:
certbot certonly --force-renew -d example.com

# The --force-renew flag tells Certbot to request a new certificate with the same domains as an existing certificate. The -d flag allows you renew certificates for multiple specific domains.

# To verify that the certificate renewed, run:
sudo certbot renew --dry-run
# If the command returns no errors, the renewal was successful

# or
certbot certonly -d certbot-demo.evermight.net
# or
certbot renew --cert-name domain1.com --dry-run
```

### Check public keys for domain

```bash
# hashes should come out the same from both cmds
openssl pkey -pubout -in privkey.pem | openssl sha256
ooenssl x509 -pubkey -in fullchain.pem -noout | openssl sha256
```



