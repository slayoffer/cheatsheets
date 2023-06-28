### Future events

```bash
docker events

```
### Return events from a date until now and future


```bash
docker events --since 2017-12-01
docker events --since 2017-12-01T12:30:00


```

### Return events from 30m ago until now and future


```bash
docker events --since 30m
docker events --since 2h10m


```

### Return last hour of events filtered by event name


```bash
docker events --since 1h --filter event=start


```

### Only return Swarm related events for networks


```bash
docker events --since 1h --filter scope=swarm --filter type=network

```