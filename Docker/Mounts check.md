### Check CT mounts

```bash
docker container inspect --format '{{json .Mounts}}' lltonebackend
```

### List containers with mounts

```bash
docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Mounts}}" --no-trunc
```

### List containers with mounts and volumes

```bash
docker ps --format '{{.ID}} {{.Names}}' | while read id name; do echo "Volumes for container $name ($id):"; docker inspect --format='{{range $vol, $path := .Mounts}}{{$vol}}: {{$path}}{{"\n"}}{{end}}' $id; done
```