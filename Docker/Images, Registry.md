### Search image on docker hub

```bash
docker search ubuntu # max show limit is 100 images

# with custom limit
docker search httpd --limit 2

# with stars filter
docker search --filter stars=10 httpd

# with official image filter
docker search --filter stars=10 --filter is-official=true httpd
```

### Search image on local server

```bash
# find images with name starting with sonar
docker images --filter "reference=sonar*"
```

### Show local images

```bash
docker images
# or
docker image list
# or
docker image ls

# list full length image IDs
docker images —no-trunc
```

### Pull image

```bash
docker image pull nginx

# pull and run
docker run nginx # will download and run latest nginx
```

### Pull & tar (for internet restricted access hosts)

```bash
docker image save alpine:latest -o alpine.tar

# then move tar to restricted host manually

# to extract and load
docker image load -i alpine.tar
# or
docker image load --input ubuntu.tar
# or
docker image load < ubuntu.tar.gz
```

### Convert CT into tar image & export

```bash
docker export container_name > testcontainer.tar
# or
docker export --output="testcontainer.tar" container_name

# to import
docker image import testcontainer.tar newimage:latest # need to set image name
```

### Commit CT to image

```bash
# recommended only for debug purposes, for other cases better use Dockerfile
docker container commit -a "Slayo" httpd customized-httpd # -a for author

# with added CMD to run after CT start
docker container commit -a "Yogesh Raheja" -c 'CMD ["httpd", "-D", "FOREGROUND"]' test webtest:v1 # -c for CMD

# start container with Apache server running
docker commit --change='ENTRYPOINT ["apachectl", "-DFOREGROUND"]' 5398ada9b000 lltv/apache-test:1.1
```

### Pipe image building

```bash
echo -e 'FROM busybox\nENTRYPOINT ["echo"]' | docker build -t myimage:vl -

docker run -it myimage:vl hello world
```

### Delete image

```bash
docker rmi image_id
# or
docker image rm image_id

# if same image has several tag copies, it will remove only tagged link, not all images
```

### Login and push to public registry

```bash
# auth creds are stored in $HOME/.docker/config.json
# https://docs.docker.com/engine/reference/commandline/login/

docker logout

docker login -u slayoffer

docker tag 7beba6bae990 slayoffer/filebeat:7.13.4

docker push slayoffer/filebeat:7.13.4
```

### Copy and tag image

```bash
# tag
docker image tag httpd:alpine httpd:customvl # creates soft link for httpd:alpine image with name customvl

# tag and push
docker image tag httpd:alpine gcr.io/company/httpd:customvl
docker image push gcr.io/company/httpd:customv
```

## Logging Into Private Registries

```bash
docker login registry.example.com
```

You can still use the `--username`, `--password`, and `--password-stdin` flags when working with custom registries. You can be logged into multiple registries simultaneously – repeat the `docker login` command as many times as you need.

### Push built image to private remote repo

```bash
# login into repo
$(aws ecr get-login ...)

# tag built image 
docker image tag willbutton:nginx 123213.kdsa.hgfdgf.amazonaws.com/willbutton:nginx

# push to remote
docker push 123213.kdsa.hgfdgf.amazonaws.com/willbutton:nginx
```

### Pull private repo image

```bash
docker pull 123213.kdsa.hgfdgf.amazonaws.com/willbutton:nginx
```

### Scan image for vulnerabilities

```bash
docker login

docker scan myapp:1.0 # will use Snyk service for scanning
```

### Create local private registry

```bash
docker run -d -p 5000:5000 --name registry --restart always registry:2

docker tag my-image localhost:5000/my-image

docker push localhost:5000/my-image

# check pushed images
curl -X GET localhost:5000/v2/_catalog
```

### Google Container Registry hub

```bash
docker pull gcr.io/kodekloud/nginx
```

