### Built in networks

```bash
# 127.0.0.11 - built in Docker DNS server

# bridge network - default
docker run --network=bridge ubuntu
# or just
docker run ubuntu

# no network
docker run --network=none ubuntu

# host network
docker run --network=host ubuntu

# overlay network - for Swarm
docker run --network=overlay ubuntu
# or
docker network create --driver overlay --subnet 10.15.0.0/16 my-overlay-network

# attachable overlay (to connect swarm network from outside swarm cluster)
docker network create --driver overlay --attachable my-overlay-network

# with encryption
docker network create --driver overlay --opt encrypted my-overlay-network



```

### Port mapping

```bash
docker container run -p 8000:5000 webapp

# publish to random port
docker container run -p 5000 webapp # 5000 for internal, random (32768-60999) for external

# can set random ports range
sudo vim ys/net/ipv4/ip_local_port_range

# limit CT to some network card only
docker container run -p 192.168.1.5:8000:5000 webapp
# or with protocol
docker container run -p 192.168.1.5:8000:5000/tcp webapp

# set to internal loopback address (wont be accessible outside of CT)
docker container run -p 127.0.0.1:8000:5000 webapp

# get internal port from Dockerfile (EXPOSE layer) and random external
docker run -P webapp # external port will change with every CT restart

# if internal port is not specified in Dockerfile
docker run -P --expose=8080 webapp

# list exposed ports in CT
docker inspect webapp # look for ExposedPorts section
```
### Iptables

```bash
# Docker auto creates DOCKER-USER and DOCKER traffic rules in host iptables
# to list rules
iptables -t nat -S DOCKER

# or
iptables -nvL -t nat
```

### Remove unused networks

```bash
docker network prune
```

### Inspect networks

```bash
# to check whether CTs are in the same network
docker network inspect bridge | less
```

### List networks

```bash
docker network ls
```

### Create network with subnet and gateway

```bash
docker network create \
	--driver bridge \ # or -d bridge
	--subnet 182.18.0.1./24 \
	--gateway 182.18.0.1 \
	isolated-network
```

### Host network

```bash
# to bind CT directly to host port 80 - works only on Linux Docker. Will not create new network interface for Docker
docker run --network host -it -d nginx
```

### Connect network

```bash
docker network connect some-network ct-name
```

### Disconnect from network

```bash
docker network disconnect some-network ct-name 
```

### Troubleshoot networking

```bash
docker run --name netshoot --rm -it nicolaka/netshoot /bin/bash
```

### Check CT network

```bash
docker inspect ct_id
# check
Networks:
```

### Embedded DNS

```bash
# use CT names instead of IPs - IP may change
```

### Hostname

```bash
# hostnames can be same on different CTs
docker container run -it --name=webapp --hostname=webapp ubuntu
# or 
docker container run -it --name=webapp -h webapp ubuntu
# default hostname is set to CT id
```

### Run CTs with same network

```bash
# add CTs to docker compose file

# mongo db
docker run -p 27017:27017 -d --name mongodb --network mongo-network \
	-e MONGO_INITDB_ROOT_USERNAME=mongoadmin \ # -e for env var
	-e MONGO_INITDB_ROOT_PASSWORD=secret \
	mongo

# mongo express
docker run -d \
	-p 8081:8081 \
	-e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
	-e ME_CONFIG_MONGODB_ADMINUSERNAME=password \
	--net mongo-network \
	--name mongo-express \
	-e ME_CONFIG_MONGODV_SERVER=mongodb \
	mongo-express
	
# or dont create network and just run second CT in same network as first one using first CT IP or probably ID
docker run --rm -it -d -p 80:80 --entrypoint "http-server" --name front sausage-store-front:v1 "/app/frontend" "-p" "80" "-proxy" "http://FIRST_CT_IP:8080"
```

### Create network with custom subnet

```bash
docker network create some-network --subnet 10.0.0.0/24
```

### Isolate network from Docker bridge

```bash
docker network create some-network --subnet 10.0.0.0/24 --internal
```

### Create router CT (gateway), add NET Cap

```bash
docker run --name gateway --network backend --network frontend -d httpd
docker run --name s1 --network backend --cap-add=NET_ADMIN -d nhttpd
docker run --name s2 --network frontend --cap-add=NET_ADMIN -d nhttpd

# then add route rules for both CTs for the outside network they need via ct exec
ip route add 10.0.0.0/24 via 10.0.1.3 # 10.0.1.3 is gateway ct ip
ip route add 10.0.1.0/24 via 10.0.0.3

# add scripts for CTs to create these route rules automatically on CT run
```

### Network namespaces

```bash
# list
ip netns # will show smth like b3165c10a92b

ip link

ip -n b3165c10a92b link
```

### MacVLan

```bash
docker network create --driver mcvlan -o parent=eth@ my-overlay-network

# or
docker network create -d macvlan --subnet 192.168.0.0/24 —-gateway 192.168.0.1 --ip-range 192.168.0.253/32 -o parent=ens18 custommacvlan

# run CT
docker run --name netshoot --rm -it --ip 192.168.0.200 --network custommacvlan nicolaka/netshoot

```

### IPVlan

```bash
# Used for multiple containers to communicate across different docker hosts. L2 Bridge.

```

### Docker networks sometimes conflict with other networks

```bash
# BIP, bridge network named "bridge"
cat /etc/docker/daemon.json

{  
  "bip": "10.15.0.1/24",
  "default-address-pools": [
	{"base": "10.20.0.0/16", "size": 24},
	{"base": "10.40.0.0/16", "size": 24}
  ]
}
```

### Debug

```bash
# use netshoot CT
docker run -it --rm --net container:debugged_ct_name \
nicolaka/netshoot ss -lnt
```