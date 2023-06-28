### Generate EC Parameters file 

```bash
openssl genpkey -genparam -algorithm EC -pkeyopt ec_paramgen_curve:secp384r1 -out EC-PARAM.pem
```

### Generate EC Keys from Parameters file  

```bash
openssl genpkey -paramfile EC-PARAM.pem -out EC-KEY.pem
```

### Generate EC Keys directly  

```bash
openssl genpkey -algorithm EC -pkeyopt ec_paramgen_curve:P-384 -out EC-KEY.pem
```

### View supported Elliptic Curves 

```bash
openssl ecparam -list_curves
# Recommended Curves: prime256v1, secp384r1, secp521r1 (identical to P-256, P-384, P-521)
```

### Inspecting Elliptic Curve (EC) Parameters file  

```bash
openssl ecparam -in EC-PARAM.pem -text -noout
```

### Inspecting Elliptic Curve (EC) Private Key file 

```bash
openssl ec -in EC-KEY.pem -text -noout
```

### Check if EC Key matches a CSR or Cert 

```bash
# Compare Public Key values to see if files match each other
openssl req -in EC-CSR.pem -noout -pubkey
openssl x509 -in EC-CERT.pem -noout -pubkey
openssl ec -in EC-KEY.pem -pubout
```

