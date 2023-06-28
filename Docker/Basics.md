### Check docker system stats & info

```bash
docker system info
# or
docker info
```

### Show running CTs

```bash
# no trunc
docker ps --no-trunc

# show CT IDs only
docker ps -q

# for all
docker ps
# or
docker container ls

# for CT size
docker ps --size

# list only last created
docker ps -l
# or several
docker ps -n 5

# for running, stopped and created
docker container ls -a

# for specific CT
docker container ls -f name=ct_name

# for short running CT IDs
docker container ls -q

# for all IDs
docker container ls -aq

# or old way
docker ps # supports same flags
# or
export PS1='> '
docker ps -a
```

### Go template

```bash
docker ps --format "ID: {{.ID}}, Name: {{.Names}}, Image: {{.Image}}, Status: {{.Status}}"

# or
docker ps --format "table {{.ID}}\t{{.Command}}\t{{.Status}}\t{{.Ports}}\t{{.Names}}"

# or just CT names
docker ps -f "table {{.Names}}" | grep gpdm
```

### Filter

```bash
docker ps –filter "name=admiring_benz"
docker ps –filter "status=running"
docker ps –filter "status=exited"
docker ps –filter "status=created"
```

### Show CT cgroup stats (dynamic)

```bash
# show all CTs stats
docker container stats
```

### Rename CT

```bash
docker container rename httpd webapp # rename httpd to webapp
```

### Pause, Unpause CT

```bash
docker container pause web

docker container unpause web
```

### Check CT processes

```bash
docker container top webapp

# or get inside CT and then
apt update && apt install procps
ps 
```

### Start CT

```bash
docker start <CT_ID>
```

### Stop CT

```bash
docker stop web01 heuristic_hugle # will gracefully stop in 10 secs

# or stop after set time
docker stop -t 60 be2848539d76 # will stop in 60 secs

# stop all CTs
docker container stop $(docker container ls -q)

# immediate stop (not recommended)
docker kill <CT_ID>

# or with term signal
docker container kill --signal=9 web01
```

### Run CT

```bash
docker run --rm --name web01 -d -p 9080:80 nginx 
# -d for detached mode (run as a service), -p for port, 9080 for external port to access internal 80 port, --name for CT name, --rm for CT remove when stopped

# run interactive for STDIN
docker run -i docker_image

# run interactive terminal for STDIN and CLI
docker run -it docker_image
```

### Run CT with CMD

```bash
docker run -d ubuntu sleep 5 # CT will terminate in 5 secs after sleep is over
```

### Run CT with user

```bash
docker run —it --user 1000:1000 busybox:1.36.O
```

## Run CT with trusty tag

```bash
docker run ubuntu:trusty
```

### Commit CTs changes to new image

```bash
# https://docs.docker.com/engine/reference/commandline/commit/
docker commit c3f279d17e0a svendowideit/testimage:version3

# commit with changes
docker commit --change "ENV DEBUG=true" c3f279d17e0a  svendowideit/testimage:version3

# commit with added cmds
docker commit --change='CMD ["apachectl", "-DFOREGROUND"]' -c "EXPOSE 80" c3f279d17e0a  svendowideit/testimage:version4
```

### Check CT OS

```bash
docker run <image_name> cat /etc/*release*
```

### Set CT restart options

```bash
# to enable CT restart
docker run -it -d --restart unless-stopped -p 80:80 nginx # will restart only if wasnt stopped manually or Docker itself was restarted

# update restart policy
docker update --restart unless-stopped redis

# set restart policy for all CTs
docker update --restart unless-stopped $(docker ps -q)
```

### Get into running CT CLI (Attach)

```bash
docker exec -it CT_ID bash # will create new bash cli process in CT
# or
docker exec -it CT_ID /bin/sh

# or attach - DOESNT CREATE NEW PROCESS! ATTACHES TO THE LAST ONE
docker attach 07b20cb5367f # check for container ID with $ docker ps

# can also use first 1-3 chars from ID
docker attach 07
```

### Execute CMD on CT

```bash
docker exec ct_name cat /etc/hosts
```

### Exit from shell and stop CT

```bash
exit
# or
Ctrl + c
```

### Detach CT

```bash
CTRL + p + q
# will show read escape sequence
```

### Logs

```bash
docker logs <ct_ID>
```


### Remove CT
```bash
# remove CT
docker ps
docker stop heuristic_hugle
docker ps -a
docker rm heuristic_hugle competent_gates elastic_ramanujan relaxed_sammet

# forceful stop and remove
docker rm -f ct_name

# remove all CTs
docker container rm $(docker container ls -aq)

# forcefull remove all CTs (even running)
docker container rm -f $(docker container ls -aq)
```



