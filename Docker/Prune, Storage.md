### Check storage

```bash
docker system df # ACTIVE column is for images with CTs

# check running CTs size
docker ps --size

# check volumes size
docker system df -v

# remove build cache
docker builder prune # add -a for all cache (including dangling cache)

# https://medium.com/homullus/how-to-inspect-volumes-size-in-docker-de1068d57f6b
```

### Prune CTs, Volumes

```bash
# for stopped CTs
docker container prune

# for non used cts, images, volumes, networks and build cache
docker system prune --all --volumes

# better add daily cron job for this
docker system prune --all --volumes -f 
# -f for force run without manual confirmation (--force) - CRON will fail without -f flag
crontab -e
0 0 * * * /usr/bin/docker system prune --all --volumes -f # daily

# The default editor for `crontab` varies by system. To open `crontab` with the `nano` editor, you can use the `VISUAL` or `EDITOR` environment variable. You can set the `VISUAL` or `EDITOR` environment variable to `nano` in your current session with one of the following commands:
export VISUAL=nano

or

bashCopy code

`export EDITOR=nano`
```

### Prune images

```bash
# for dangling images
docker image prune

# for all images not attached to CTs
docker image prune -a

# check dangling images
docker images --filter "dangling=true"
```

### Prune Yolo

```bash
docker run -d --restart=unless-stopped --name cleanup \
-v /var/run/docker.sock:/var/run/docker.sock \
docker /bin/sh -c \
"while true; do docker system prune -f; sleep 1h; done"

# for swarm
docker service create --mode global --name cleanup \
--mount type=bind,src=/var/run/docker.sock,\
dst=/var/run/docker.sock \
docker /bin/sh -c \
"while true; do docker system prune -f; sleep 1h; done"
```