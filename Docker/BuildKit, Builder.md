### Enable

```bash
export DOCKER_BUILDKIT=1

docker build -t your_image .

cat /etc/docker/daemon.json

{ "features": {"buildkit": true} }
```

### Prune

```bash
docker builder prune --keep-storage=1GB --filter until=72h

cat /etc/docker/daemon.json

{
"builder": {
"gc": {
"enabled": true,
"policy": [
{"keepStorage": "512MB", "filter": ["unused-for=168h"]]},
{"keepStorage": "30GB", "all": true}
]
} } }
```