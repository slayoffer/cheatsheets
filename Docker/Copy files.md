### Copy to CT

```bash
docker container cp /tmp/web.conf webapp:/etc/web.conf

# or without file name
docker container cp /tmp/web.conf webapp:/etc/ # etc dir on ct must exist!

# copy dir
docker container cp /tmp/app/ webapp:/opt/app

# also can copy a file from a stopped container but not from removed one
```

### Copy from CT

```bash
docker container cp webapp:/root/dockerhost /tmp/

# or with CT id
docker container cp 89683681:/web.conf /tmp/
```

