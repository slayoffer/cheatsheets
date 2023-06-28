### Commit flow

```bash
docker create --name nginx_base -p 80:80 nginx:alpine
```

### Start the Container

```bash
docker start nginx_base
```

### Modify the Running Container

```bash
<html>
<head>
<title>Hello World</title>
</head>
<body>
<h1>Hello World!</h1>
</body>
```

Then save the file and return to the command line. We will use the docker cp command to copy this file onto the running container.

```bash
docker cp index.html nginx_base:/usr/share/nginx/html/index.html
```

### Create an Image From a Container

```bash
docker commit nginx_base
```

### Tag the Image

```bash
docker tag 0c17f0798823 hello_world_nginx
```

### Create Images With Tags

```bash
docker commit nginx_base hello_world_nginx
```

### Delete the Original Container

```bash
docker ps
docker stop nginx_base
docker rm nginx_base
```

### Option A: Set Authorship

```bash
docker inspect hello_world_nginx | grep Author
```

So if we use the author option on the docker commit command, we can set the value of the author field.

```bash
docker commit --author amit.sharma@sentinelone.com nginx_base authored
```

### Option B: Create Commit Messages

```bash
docker commit --message 'this is a basic nginx image' nginx_base mmm

docker history mmm
```

### Option C: Commit Without Pause

```bash
docker commit --pause=false nginx_base wo_pause
```

If you don’t pause the container, you run the risk of corrupting your data.

For example, if the container is in the midst of a write operation, the data being written could be corrupted or come out incomplete. That is why, by default, the container gets paused before the image is created.

### Option D: Change Configuration

The last option I want to discuss is the -c or –change flag. This option allows you to set the configuration of the image.

You can change any of the following settings of the image during the commit process:

- CMD
- ENTRYPOINT
- ENV
- EXPOSE
- LABEL
- ONBUILD
- USER
- VOLUME
- WORKDIR

Nginx’s original docker file contains the following settings:

- CMD [“nginx”, “-g”, “daemon off;”]
- ENV NGINX_VERSION 1.15.3
- EXPOSE 80

So we will just play with one of those for a moment. The NGINX_VERSION and EXPOSE could cause issues with container startup, so we will mess with the command line (CMD) executed by the container.

Nginx allows us to pass the -T command line argument that will dump its configuration to standard out. Let’s make an image with an alternate CMD value as follows:

```bash
docker commit --change='CMD ["nginx", "-T"]' nginx_base conf_dump
```

Now stop the nginx_base container with this command:

```bash
docker stop nginx_base
```

And start a new container from the image we just created:

```bash
docker run --name dumper -p 80:80 conf_dump
```

The configuration for the [nginx](https://en.wikipedia.org/wiki/Nginx) process will be dumped to standard out when you execute this command. You can scroll back several pages to find the command we executed.