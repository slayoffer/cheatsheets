### Force update

```bash
# will recreate CTs, without --force will not restart CTs
docker service update --force firefly

# or with some other option
docker service update --update-parallelism 5 --force firefly

docker service inspect --pretty firefly

```

### Order update

```bash
docker service update --update-order start-first firefly # or can be stop-first - which is used mostly for DBs

```