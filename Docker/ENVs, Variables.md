### Run CT with var

```bash
docker run -e APP_COLOR=blue webapp-ct
```

### Check vars in CT

```bash
docker inspect ct-name
# check
Config:
	Env:
		APP_COLOR=green
```

### Check env field from within CT

```bash
docker exec -it mysql-db env
```

### Check ENV vars inside CT

```bash
docker exec dkc-hercules-backend /usr/bin/env
```

### Change ENVs inside CT

```bash
docker exec dkc-hercules-backend /usr/bin/env SMTP__PASSWORD=rjbmnecppnjjatcy
```

