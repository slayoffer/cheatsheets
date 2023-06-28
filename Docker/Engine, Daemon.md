### Override config

```bash
sudo vim /etc/systemd/system/docker.service.d/override.conf

 override.conf

# add
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd --config-file /etc/docker/daemon.json

# or change original config at
sudo vim /lib/systemd/system/docker.service

# add smth like
ExecStart=/usr/bin/dockerd --containerd=/run/containerd/containerd.sock --config-file /etc/docker/daemon.json
```

### Docker socket errors

```bash
# if gives error "can't create unix socket /var/run/docker.sock: is a directory"
sudo systemctl stop docker

# check if its really a dir
ls -la /var/run/docker.sock

sudo rm -fr /var/run/docker.sock

sudo systemctl start docker
# check if it is socket now
ls -la /var/run/docker.sock
```


### Edit current Docker systemd config

```bash
sudo systemctl edit docker
```

### Daemon.json

```bash
sudo vim /etc/docker/daemon.json

{
  "insecure-registries": [
    "devcraft.app:8082",
    "devcraft.app:8083"
  ],
  "tls": true, # not needed for TLS protected docker host, only needed for client
  "tlsverify": true, # needed for TLS protected docker host
  "tlscacert": "/etc/docker/ssl/ca.pem",
  "tlscert"  : "/etc/docker/ssl/server-cert.pem",
  "tlskey"   : "/etc/docker/ssl/server-key.pem",
  "hosts"    : ["unix:///var/run/docker.sock", "tcp://0.0.0.0:2376"], # can use "fd://" instead of "unix..."
  "bip"      : "172.21.0.1/16", # This setting defines the IP address and subnet mask for the Docker bridge network
  "default-address-pools": # This is an array of objects representing the default address pools for the Docker daemon
  [
    {
	  "base": "172.22.0.0/16", # This specifies the base IP address for the pool, along with its subnet mask
	  "size": 26 # This specifies the size of each subnet in the pool. The size is represented as the number of bits for the subnet mask
	}
  ],
  "default-runtime": "nvidia", # default docker runtime is "runc"
  "default-shm-size": "1G",
  "storage-driver": "overlay2",
  "runtimes": {
     "nvidia": {
         "path": "/usr/bin/nvidia-container-runtime",
         "runtimeArgs": []
     }
  }
}

# if use daemon.json with docker override.conf add daemon.json to systemctl
sudo vim /etc/systemd/system/docker.service.d/override.conf
# add
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd --config-file /etc/docker/daemon.json
```

### Update

```bash
# First check from which repository you have installed your docker
sudo apt list | egrep -i "docker" | egrep -i "installed"

sudo apt update

# Then purge packages that are old:
sudo apt purge -y docker.io docker-engine

# then install docker from scratch
# Uninstall old versions
# Older versions of Docker went by the names of docker, docker.io, or docker-engine, you might also have installations of containerd or runc. Uninstall any such older versions before attempting to install a new version:
sudo apt remove docker docker-engine docker.io containerd runc

# apt-get might report that you have none of these packages installed.
# Images, containers, volumes, and networks stored in /var/lib/docker/ aren’t automatically removed when you uninstall Docker. If you want to start with a clean installation, and prefer to clean up any existing data, read the uninstall Docker Engine section.

 sudo apt update
 sudo apt install \
    ca-certificates \
    curl \
    gnupg
    
# Add Docker’s official GPG key:
sudo mkdir -m 0755 -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Use the following command to set up the repository:
echo \
"deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
"$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \

sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

# Receiving a GPG error when running apt-get update?
# Your default umask may be incorrectly configured, preventing detection of the repository public key file. Try granting read permission for the Docker public key file before updating the package index:
sudo chmod a+r /etc/apt/keyrings/docker.gpg

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo docker run hello-world
```