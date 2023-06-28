### Set Autolock (encrypt cluster)

```bash
docker swarm init --autolock=true # will show key - SAVE IT!

# or for existing cluster
docker swarm update --autolock=true

# to unlock
docker swarm unlock

# show lock key
docker swarm unlock-key
```
