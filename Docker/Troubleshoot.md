### Docker daemon

```bash
docker system info

docker system events --since 60m

# for CT
docker system events --filter 'container=webapp'

# if remote access to docker host is used then check host var
export DOCKER_HOST="tcp://192.168.1.10:2375" # 2376 for encrypted docker host

systemctl status docker

journalctl -u docker.service

# check disk space and delete not used CTs
df -h
docker container prune
docker image prune
```
### Logs

```bash
journalctl -u docker.service
# or
cat /var/log/messages # for CentOS
cat /var/log/syslog # for Debian
# or
cat var/log/daemon.log
# or
cat /var/log/docker.log # for old Linux
```

### Logs of CT

```bash
tail -30 /var/log/messages # for CentOS

# or
docker logs <ct_ID>
# or
docker logs loving_golick # CT given name

# dynamic logging
docker logs -f <ct_ID> # -f for follow
# tail logging
docker logs --tail <ct_ID>
# or
tail -30 /var/log/messages # for CentOS

# Logging driver
docker ps # get CT id
# check CT dir for this id
cd /var/lib/docker/containers; ls
# cat CT's id
cat /var/lib/docker/containers/lakjdsfho45u0543u.json
```

### Daemon logs

```bash
sudo vim /etc/docker/daemon.json
# set 
"debug": true,

# reload docker
sudo systemctl reload docker
# or
ps -ef | grep -i dockerd
kill -SIGHUP 3664

docker system info # check docker debug mode section to check debug mode setting

# print docker deamon logs on screen
dockerd # used to start Docker in foreground for debugging

# for additional details use
dockerd --debug

# Docker CLI ulistens only to local Unix socket /var/run/docker.sock
# to access from remote server set on host server
export DOCKER_HOST="tcp://192.168.1.10:2375" # 2376 for encrypted docker host
# then access from remote
dockerd --debug --host=tcp://192.168.1.10:2375 # 2375 is default Docker port
```

### Inspect stats search

```bash
# check log driver setting
docker container inspect -f '{{.HostConfig.LogConfig.Type}}’ nginx
# or inspect CT in general
docker container inspect nginx
```



### Local debug

```bash
docker exec -it <CT_ID> bash
```

### Show CT hardware resource (cgroup) usage

```bash
# show all CTs stats
docker container stats

# --no-stream for non dynamic (snapshot) view
docker stats --no-stream ct_name 
```

### Check CT processes

```bash
docker container top webapp
```



### Logging driver config

```bash
sudo systemctl stop docker
sudo vim /etc/docker/daemon.json

# set default logging
"log-driver": "json-file"
sudo systemctl start docker

# set syslog logging
"log-driver": "syslog"
# or
"log-driver": "journald"

# set logging for AWS
"log-driver": "awslogs"
"log-opt": {
	"awslogs-region": "us-east-1"
}

# disable logging
"log-driver": "none"

# set AWS creds
export AWS_ACCESS_KEY_ID=<>
export AWS_SECRET_ACCESS_KEY=<>
export AWS_SESSION_TOKEN=<>

# to override config log driver values
docker run -d --log-driver=json-file nginx

# inspect LogConfig value
docker container inspect nginx
# or
docker container inspect -f '{{.HostConfig.LogConfig.Type}}’ nginx
```



