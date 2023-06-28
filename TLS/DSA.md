### Generate DSA Parameters File

```bash
openssl dsaparam -out DSA-PARAM.pem 1024
```

### Generate DSA Keys file with Parameters file  

```bash
openssl gendsa -out DSA-KEY.pem DSA-PARAM.pem
```

### Generate DSA Parameters and Keys in one File  

```bash
openssl dsaparam -genkey -out DSA-PARAM-KEY.pem 2048
```

### Inspecting DSA Parameters file  

```bash
openssl dsaparam -in DSA-PARAM.pem -text -noout
```

### Inspecting DSA Private Key file 

```bash
openssl dsa -in DSA-KEY.pem -text -noout
```

