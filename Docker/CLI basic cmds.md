## Manage images

### `docker build`


```bash
docker build [options] .
  -t "app/container_name"    # name
```

Create an `image` from a Dockerfile.

### `docker run`

```bash
docker run [options] IMAGE
  # see `docker create` for options
```

Run a command in an `image`.

## Manage containers

### `docker create`

```bash
docker create [options] IMAGE
  -a, --attach               # attach stdout/err
  -i, --interactive          # attach stdin (interactive)
  -t, --tty                  # pseudo-tty
      --name NAME            # name your image
  -p, --publish 5000:5000    # port map
      --expose 5432          # expose a port to linked containers
  -P, --publish-all          # publish all ports
      --link container:alias # linking
  -v, --volume `pwd`:/app    # mount (absolute paths needed)
  -e, --env NAME=hello       # env vars
```

#### Example

```bash
docker create --name app_redis_1 \
  --expose 6379 \
  redis:3.0.2
```

Create a `container` from an `image`.

### `docker exec`

```bash
docker exec [options] CONTAINER COMMAND
  -d, --detach        # run in background
  -i, --interactive   # stdin
  -t, --tty           # interactive
```

#### Example

```bash
docker exec app_web_1 tail logs/development.log
docker exec -t -i app_web_1 rails c
```

Run commands in a `container`.

### `docker start`

```bash
docker start [options] CONTAINER
  -a, --attach        # attach stdout/err
  -i, --interactive   # attach stdin

docker stop [options] CONTAINER
```

Start/stop a `container`.

### `docker ps`

```bash
docker ps
docker ps -a
docker kill $ID
```

Manage `container`s using ps/kill.

## Images

### `docker images`

```bash
docker images
  
docker images -a   # also show intermediate
```

Manages `image`s.

### `docker rmi`

```bash
docker rmi b750fe78269d
```

Deletes `image`s.

# Dockerfile

### Inheritance

```bash
FROM ruby:2.2.2
```

### Variables

```bash
ENV APP_HOME /myapp
RUN mkdir $APP_HOME
```

### Initialization

```bash
RUN bundle install
WORKDIR /myapp
VOLUME ["/data"]
# Specification for mount point
ADD file.xyz /file.xyz
COPY --chown=user:group host_file.xyz /path/container_file.xyz
```

### Onbuild

```bash
ONBUILD RUN bundle install
# when used with another file
```

### Commands

```bash
EXPOSE 5900
CMD    ["bundle", "exec", "rails", "server"]
```

### Entrypoint

```bash
ENTRYPOINT ["executable", "param1", "param2"]
ENTRYPOINT command param1 param2
```

Configures a container that will run as an executable.

```bash
ENTRYPOINT exec top -b
```

This will use shell processing to substitute shell variables, and will ignore any `CMD` or `docker run` command line arguments.

### Metadata

```bash
LABEL version="1.0"
LABEL "com.example.vendor"="ACME Incorporated"
LABEL com.example.label-with-value="foo"
LABEL description="This text illustrates \
that label-values can span multiple lines."
```

# Docker Compose

### Basic example

```yml
version: '3'

services:
  web:
    build: .
    # build from Dockerfile
    context: ./Path
    dockerfile: Dockerfile
    ports:
     - "5000:5000"
    volumes:
     - .:/code
  redis:
    image: redis
```

### Commands

```bash
docker-compose start
docker-compose stop
docker-compose pause
docker-compose unpause
docker-compose ps
docker-compose up
docker-compose down
```

### Building

```yml
web:
  # build from Dockerfile
  build: .
  # build from custom Dockerfile
  build:
    context: ./dir
    dockerfile: Dockerfile.dev
  # build from image
  image: ubuntu
  image: ubuntu:14.04
  image: tutum/influxdb
  image: example-registry:4000/postgresql
  image: a4bc65fd
```

### Ports

```yml
  ports:
    - "3000"
    - "8000:80"  # guest:host
  # expose ports to linked services (not to host)
  expose: ["3000"]
```

### Logging

```yml
logging:
  options:
	max-size: "10m"
	max-file: "3"
```

### Commands

```yml
  # command to execute
  command: bundle exec thin -p 3000
  command: [bundle, exec, thin, -p, 3000]
  
  # override the entrypoint
  entrypoint: /app/start.sh
  entrypoint: [php, -d, vendor/bin/phpunit]
```

### Environment variables

```yml
# environment vars
  environment:
    RACK_ENV: development
  environment:
    - RACK_ENV=development
# environment vars from file
  env_file: .env
  env_file: [.env, .development.env]
```

### Dependencies

```yml
  # makes the `db` service available as the hostname `database`
  # (implies depends_on)
  links:
    - db:database
    - redis
  # make sure `db` is alive before starting
  depends_on:
    - db
```

### Other options

```yml
  # make this service extend another
  extends:
    file: common.yml  # optional
    service: webapp
  volumes:
    - /var/lib/mysql
    - ./_data:/var/lib/mysql
```

## Advanced features

### Labels

```yml
services:
  web:
    labels:
      com.example.description: "Accounting web app"
```

### DNS servers

```yml
services:
  web:
    dns: 8.8.8.8
    dns:
      - 8.8.8.8
      - 8.8.4.4
```

### Devices

```yml
services:
  web:
    devices:
    - "/dev/ttyUSB0:/dev/ttyUSB0"
```

### External links

```yml
services:
  web:
    external_links:
      - redis_1
      - project_db_1:mysql
```

### Hosts

```yml
services:
  web:
    extra_hosts:
      - "somehost:192.168.1.100"
```