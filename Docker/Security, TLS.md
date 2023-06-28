### Set remote TLS host

```bash
# put key.pem, cert.pem and ca.pem in ~/.docker dir and then set vars
export DOCKER_HOST=tcp://212.233.92.30:2376 
export DOCKER_TLS_VERIFY=1

# or set other dir for certs
export DOCKER_CERT_PATH=/home/gitlab-runner/remote-certs

# run on local
docker run busybox

# check on remote
docker ps -a
```

### Run CT on remote non-tls host

```bash
docker -H=tcp://192.168.1.10:2375
# or
docker --host=tcp://192.168.1.10:2375
# or
docker -H=tcp://192.168.1.10:2375 run nginx # 2375 is default Docker port (unsecure)
```

###  Set remote host for debug

```bash
# allow remote acccess from 192.168.1.10
dockerd --debug --host=tcp://192.168.1.10:2375

# better set with tls but need to make certificates frist
dockerd --debug --host=tcp://192.168.1.10:2376 \ # 2376 is for encrypted traffic
--tls=true \
--tlscert=/var/docker/server.pem \
--tlskey=/var/docker/serverkey.pem
```

### Process of setting up Host and client

```bash
# https://github.com/AlexisAhmed/DockerSecurityEssentials

# 1) Disable root access on Docker host server

# 2) Add "docker" user to groups docker and sudo

# 3) Add only trusted users to docker group, because they will have all the Docker permissions even without sudo access

# 4) Move all keys and certs from ~/.docker to /etc/docker/ssl dir
# if dont move to /etc, then change owner to root
sudo chown -R root:root ~/.docker

sudo mkdir /etc/systemd/system/docker.service.d
sudo vim /etc/systemd/system/docker.service.d/override.conf
# add
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -D -H unix:///var/run/docker.sock --tlsverify --tlscert=/home/slayo/.docker/server-cert.pem --tlscacert=home/slayo/.docker/ca.pem --tlskey=home/slayo/.docker/server-key.pem -H tcp://0.0.0.0:2376
# restart docker
sudo systemctl deamon-reload
sudo systemctl restart docker
# check ports
netstat -antp

# for client server copy key.pem, cert.pem and ca.pem to ~/.docker and run
dockerd --tlsverify \
    --tlscacert=ca.pem \
    --tlscert=cert.pem \
    --tlskey=key.pem \
    -H tcp://10.0.0.9:2376 # domain from server cert domains list

# or via daemon.json
sudo vim /etc/docker/daemon.json
{
  "tlsverify": true,
  "tls": true, # not needed in case full TLS protection
  "tlscacert": "/data/certs/ca.pem",
  "tlscert"  : "/data/certs/server-cert.pem",
  "tlskey"   : "/data/certs/server-key.pem",
  "hosts"    : ["fd://", "tcp://0.0.0.0:2376"]
  "default-runtime": "nvidia",
  "default-shm-size": "1G",
  "storage-driver": "overlay2",
  "runtimes": {
     "nvidia": {
         "path": "/usr/bin/nvidia-container-runtime",
         "runtimeArgs": []
     }
  }
}

# can move these settings to config file
vim /etc/docker/daemon.json

# or set ENVs
export DOCKER_TLS_VERIFY="1"
export DOCKER_HOST="tcp://IP:2376"
export DOCKER_CERT_PATH="/home/slayo/.docker"

# or add them to .bashrc
export DOCKER_HOST="tcp://10.0.0.9:2376"
export DOCKER_TLS_VERIFY=1
export DOCKER_CERT_PATH="/home/slayo/.docker"
```

### Change namespace (to protect from CT root user brakeout)

```bash
# check subuid
cat /etc/subuid

sudo vim /etc/systemd/system/docker.service.d/override.conf
# add
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -D -H unix:///var/run/docker.sock --tlsverify --tlscert=/home/slayo/.docker/server-cert.pem --tlscacert=home/slayo/.docker/ca.pem --tlskey=home/slayo/.docker/server-key.pem --userns-remap="default" -H tcp://0.0.0.0:2376
# restart docker
sudo systemctl deamon-reload
sudo systemctl restart docker

# dockremap should be in subuid
cat /etc/subuid
```

### Disable communication between CTs

```bash
sudo vim /etc/systemd/system/docker.service.d/override.conf
# add
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd -D -H unix:///var/run/docker.sock --tlsverify --tlscert=/home/slayo/.docker/server-cert.pem --tlscacert=home/slayo/.docker/ca.pem --tlskey=home/slayo/.docker/server-key.pem --userns-remap="default" -cc="false" -H tcp://0.0.0.0:2376
# restart docker
sudo systemctl deamon-reload
sudo systemctl restart docker

# to check
sudo ./docker-bench-security.sh
```

