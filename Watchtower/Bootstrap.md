## Run Watchtower

Watchtower can be easily deployed by executing a simple docker run command.

```
docker run --name watchtower -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower
```

## Run Watchtower in debug mode

You might wonder why there is no log output apart from the welcome message. If you want to increase the logging level or watchtower, you simply just add an argument.

```
docker run --name watchtower -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --debug
```

## Run Watchtower only once, in debug mode

```
docker run --name watchtower -v /var/run/docker.sock:/var/run/docker.sock containrrr/watchtower --run-once --debug
```

## Exclude Container from Watchtower

```
docker run -d --label=com.centurylinklabs.watchtower.enable= false nginx
```

## Scheduled Updates and clean up old images

```
docker run --name watchtower -v /var/run/docker.sock:/var/run/docker.sock --restart unless-stopped containrrr/watchtower --schedule "0 0 4 * 
```