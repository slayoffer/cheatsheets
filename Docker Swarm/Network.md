### Ingress network

```bash
docker service create \
 --rep1icas=2 \
 -p 80:5000 \
 my—web—server
```

### Overlay network

```bash
docker network create --driver overlay --subnet 10.0.9.0/24 my-overlay-network
```

### Create attachable network

```bash
# other Swarm CTs will be able to join this network without permission from Master node
docker network create --driver overlay --attachable verse

```

### Subnet collisions

```bash
docker swarm init \
--default-addr-pool 10.20.0.0/16 \
--default-addr-pool 10.40.0.0/16 \
--default-addr-pool-mask-length 24
```

### Debug

```bash
# use netshoot CT
docker run -it --rm --net container:debugged_ct_name \
nicolaka/netshoot ss -lnt
```