### Restart CT

```bash
docker restart container_name

# if you want to see the output of your command then you should add -ai options:
docker start -ai container_name
```

### Compose

```yml
deploy:
  restart_policy:
    condition: on-failure
    delay: 5s
    max_attempts: 3
    window: 120s
```

```

### Policies

```bash
docker container run --restart=no ubuntu # default
# or
docker container run ubuntu

docker container run --restart=on-failure ubuntu

docker container run --restart=always ubuntu # if was manually stopped will restart only if docker deamon restarts

docker container run --restart=unless_stopped ubuntu # same as always but will not restart if was manually stopped
```

### Check CT policy

```bash
docker container inspect webapp
```

### Update CT policy

```bash
docker container update --restart always httpd

# update all CTs
docker container update --restart unless-stopped $(docker container ls -q)
```

