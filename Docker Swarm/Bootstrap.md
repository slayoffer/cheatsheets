### Init

```bash
# on master (manager) node
docker swarm init

# check
docker system info | grep -i swarm

# init with specific host ip
docker swarm init --advertise-addr 192.168.1.38

# on worker (slave) nodes
docker swarm join --token token
```

### List cluster nodes

```bash
docker node ls
```

### Inspect node

```bash
docker node inspect manager1 --pretty
```

### Leave cluster

```bash
docker swarm leave

# then remove this node from swarm nodes list if needed
docker node rm node_name
```

### Generate manager token

```bash
# generate master token
docker swarm join-token manager
```

### Generate worker token

```bash
docker swarm join-token worker
```

### Promote node to manager role

```bash
docker node promote node_name
```

### Demote node

```bash
docker node demote node_name
```

### Promote

```bash
# to start cluster with new master node
docker swarm init --force-new-cluster --advertise-addr ip_addr
# or just
docker swarm init --force-new-cluster

# promote node to become master (run on node)
docker node promote node_name
```

### Drain node (for maintenance or to keep it only for manager tasks)

```bash
# for example to make node to only do cluster manager tasks (not running CTs on it, etc)
docker node update --availability drain node_name
```

### Undrain node (make active)

```bash
docker node update --availability active node_name
```

### Remove node from cluster

```bash
# on manager node
docker node update --availability drain node_name

# on worker node
docker system info | grep -i swarm # should be active

docker swarm leave

docker system info | grep -i swarm # should be inactive

# on manager node
docker node rm worker_name
```