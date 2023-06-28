### Create config

```bash
# create onject
docker config create nginx-conf /tmp/nginx.conf

# attach object
docker service create --replicas=4 --config nginx-conf nginx # will attach to every CT at /nginx.conf

# attach with path
docker service create --replicas=4 --config src=nginx-conf,target="/etc/nginx/nginx.conf" nginx



```

### Remove config

```bash
docker service update --config-rm nginx-conf nginx

docker config rm nginx-conf


```

### Update config on service (rotate object)

```bash
# create new
docker config create nginx-conf-new /tmp/nginx-new. conf

# remove old and add new
docker service update --config-rm nginx-conf --config-add nginx-conf-new nginx

```
