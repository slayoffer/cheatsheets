### Start services

```bash
# service will run on replicas count nodes
docker service create --replicas 3 nginx:alpine
# or
docker service create --mode=replicated --replicas=2 webapp

# with ct name
docker service create --replicas 3 --name web-server -p 80:80 nginx:alpine

# with network
docker service create --network my-overlay-network my-web-service

# with explicit port mapping
docker service create --publish published=80,target=5000 my-web-server

# with dns
docker service create -name=my-web-server —dns=8.8.8.8 —network=my-overlay my-web-server


# with port types
docker service create --publish published=80,target=5000,protocol=udp my-web-server




# with image
docker service update -p 80:80 --image=web:2 web # rolling update (one by one)

# with delay
docker service update -p 80:80 --update-delay 60s --image=web:2 web # will wait 60 secs before updateing each instance

# in parallel
docker service update -p 80:80 --update-parallelism 3 --image=web:2 web

# set what to do on update failure
docker service update -р 80:80 \
  --update-failure-action pause|continue|rollback \ # need to choose one
  --image=web:2 web

# rollback CTs to older version
docker service update --rollback web
```

### Add replicas

```bash
docker service update --replicas 4 my-web-server
# or
docker service scale webapp=4
```

### Add ports

```bash
# 4789 should be opened to allow the overlay and ingress network traffic

# 7946 port should be opened to allow communication among nodes/Container Network Discovery

# 2377 should be opened for Swarm

docker service update service_id --publish-add 5000:80
```

### Remove service

```bash
docker service rm service_id
```

### Inspect service

```bash
docker service inspect firstservice --pretty
```

### List services

```bash
docker service ls
```

### List service containers

```bash
docker service ps service_id
```

### Show logs

```bash
docker service logs service_name
```

### Global services

```bash
# service will run on all nodes
docker service create --mode global my-service
```

### Add labels

```bash
docker node update --label-add type=cpu-optimized worker1

docker node update --label-add type=memory-optimized worker2

docker node update --label-add type=gp worker3


```

### Add constraints

```bash
docker service create --constraint=node.labels.type==cpu-optimized batch-processing

docker service create --constraint=node.labels.type==memory-optimized realtime-analytics

# NOT constraint
docker service create --constraint=node.labels.type!=memory-optimized web

# worker role constraint
docker service create --constraint=node.role==worker web




```

### Encryption

```bash
# By default, all swarm service management traffic is encrypted using AES algorithm
docker network create --driver overlay --opt encrypted my-overlay-network
# or
docker network create --driver overlay -o encrypted my-overlay-network
```
