### Install

```bash
# verify that Docker Compose is installed correctly by checking the version.
docker compose version
```

### Down and up from scratch

```bash
# total wipe with volumes, but keep images
docker compose down --volumes --remove-orphans

# recreate with changes made in config files, .envs, etc
docker compose up --force-recreate

# total wipe, but keep network
docker compose down --volumes --remove-orphans --network dkc-hercules
docker compose down --volumes --remove-orphans --network=none

# total wipe with images
docker compose down --volumes --remove-orphans --rmi # add “local” to remove only images that don’t have a custom tag (“local”|”all”)
```

### To recreate only one container in a Docker Compose file, follow these steps:

```bash
docker compose stop filebeat

docker compose rm -f filebeat

docker compose up -d --no-deps filebeat
```

### Find CTs compose file

```bash
# look for compose file location label
docker inspect --format='{{json .Config.Labels}}' ct_name
```

### Run

```bash
docker compose up -d
```

### Stop

```bash
docker compose stop
```

### Show processes in compose CTs

```bash
docker compose top
```

### Show logs

```bash
docker compose logs -f
```

### Show compose CTs

```bash
docker compose ps
```

### Compose looks for following files by default

```bash
docker-compose.yml
docker-compose.yaml
compose.yml
compose.yaml

# or use custom file name
docker-compose -f /path/to/your-file.yml up
```

### Set project name

```bash
# set dir name or use -p flag
docker-compose -p CUSTOM_PROJECT_NAME up -d

# or set in yaml
environment:
  - COMPOSE_PROJECT_NAME=CUSTOM_PROJECT_NAME
```

### Start/stop compose

```bash
docker-compose -f mongo.yaml up

docker-compose -f mongo.yaml down
```

### Watch and sync with changes in files or recreate

```bash
git clone [https://github.com/dockersamples/avatars.git](https://github.com/dockersamples/avatars.git)

cd avatars

docker compose up --wait && docker compose alpha watch

# open [http://localhost:5735](http://localhost:5735/) in your browser

# example
services:
  web:
    build: .
    x-develop:
      watch:
        - action: sync
          path: ./web # will sync files in CTs /src/web dir with local ./web dir
          target: /src/web
        - action: rebuild
          path: package.json # will rebuild image in case changes in local package.json file
```