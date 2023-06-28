### Create Image

```bash
mkdir images
cd images/
vim Dockerfile

# then paste below content into Dockerfile
FROM ubuntu:22.02 # dont ever use :LATEST tag !
MAINTAINER Slayo <slayoffer@gmail.com>
# Skip prompts
ARG DEBIAN_FRONTEND=noninteractive
# Update packages
RUN apt update; apt dist-upgrade -y
# Install packages
RUN apt install -y apache2 vim-nox
# Set entrypoint
ENTRYPOINT apache2ctl -D FOREGROUND
```

### Entrypoint

```bash
FROM Ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]

docker run ubuntu-sleeper-ct 10 # will exec sleep 10 on CT start
# if 10 wasnt specified on CT start it will use default (5) from CMD entry

# to change default entrypoint
docker run --entrypoint new-sleep-cmd ubuntu-sleeper-ct 10
```

### ARG

```bash
ARG VERSION
СОРУ target/sausage—store—${VERSION}.jar ./sausage—store.jar

docker build . --build-arg 1.0 slayoffer/test-image:1.0
```

### WORKDIR

```bash
# WORKDIR works only for CMD & ENTRYPOINT
```

### Dumb init (or can use Tinit) to run several processes in CT

```bash
# great tool for CT PIDs to work without zombie processes and so on
ENTRYPOINT ["dumb—init", "java", "—jar", "—Dmyserver. bindPort=8080", "./sausage—store."]

# check docs how to install dumb init or Tinit
```

### Add or replace text in file

```bash
RUN echo "Some line to add to a file" >> /etc/sysctl.conf

# If you wish to replace some characters or similar you can work this out with sed by using e.g. the following:
RUN sed -i "s|some-original-string|the-new-string |g" /etc/sysctl.conf
```

### Build image

```bash
docker build . -f Dockerfile -t slayoffer/test-image:1.0 # -f is not needed if Docker file is named Dockerfile

# with custom file and outside (non PWD) dir
docker build -f /opt/myapp/Dockerfile.dev /opt/myapp -t webapp
```

### Build from remote repo

```bash
docker bui1d https://github.com/myaccount/myapp

# or with branch
docker bui1d https://github.com/myaccount/myapp#branch_name

# or with folder
docker bui1d https://github.com/myaccount/myapp:/dir
docker build https://github.com/kk/dca.git#dev:docker

# or with custom Dockerfile name (for example for Dev or Test image)
docker bui1d -f Dockerfile.dev https://github.com/myaccount/myapp # -f for file
```

### Build Context & ignore

```bash
# set build dir
docker build /tmp/docker -t webapp # will look for dockerfile in /tmp/docker

# set build dir and docker file
docker build -f /opt/myapp/Dockerfile.dev /opt/myapp -t webapp

# get rid of all unnecessary files before building or ignore them
vim .dockerignore
```

### Build cache

```bash
# put most changeable layers in bottom in Dockerfile
# put least changeable layers topmost in Dockerfile

# Cache Busting - ALWAYS combine apt update and apt install in same RUN directive
RUN apt update && apt install -y \ # sort packages alphabetically and put each on new line
python \
python3-pip \
python-dev=20.0.2 # Version Pinning

# disable cache when building
–no-cache=true
```

### Difference between COPY and ADD

```bash
# ADD can extract tarballs
ADD app.tar.xz /testdir

# ADD can download files
ADD http://app.tar.xz /testdir
RUN tar -xJf /testdir/app.tar.xz -C /tmp/app
RUN make -C /tmp/app

# but MUCH better use RUN for download and extract (less layers)
RUN curl http://app.tar.xz \
| tar -xc) /testdir/file.tar.xz \
&& yarn build \
&& rm /testdir/file.tar.xz

# Use COPY always when possible and ADD only for unzip case (above), but can also use RUN for unzip
```

### Local and remote building

```bash
# all dockerfiles are always built in this dir
/var/lib/docker/tmp/docker-builder_id

# for remote building use DOCKER_HOST var or -h flag (check Remote Access.md)
```

### Add user to Dockerfile for rootless container (better security)

```bash
RUN adduser \
--disabled-password \
--gecos ""
--home "/app" \
--no-create-home \
--uid 5001 \
nodejs
RUN groupadd -r nodejs
RUN chown -R nodejs:nodejs /app
USER nodejs
```

### Website CT example

```bash
vim some_desriptive_name.Dockerfile

FROM nginx:latest
COPY html /usr/share/nginx/html

RUN echo "Hi will" > /root/will.txt

docker build -t webserver:latest .
docker images | grep webserver
```





