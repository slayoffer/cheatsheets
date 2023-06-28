### Volume list

```bash
docker volume ls
```

### Find Volume local path (mount point) and other details

```bash
docker volume inspect nginx-data
```

### Check where named volume belongs to

```bash
docker ps -a --filter volume=nextcloud
```

### Create new volume folder

```bash
# will be created in /var/lib/docker/volumes
docker volume create data_volume 

cd /var/lib/docker/volumes/data_volume

docker run -v data_volume:/var/lib/mysql mysql # volume mount
```

### Named Bind Volume

```bash
docker run --mount type=bind,source=/data/mysql,target=/var/lib/mysql mysql

# compose
version: '3.7'
volumes:
bind-vol:
driver: local
driver_opts:
type: none
o: bind
device: /home/user/host-dir
services:
app:
volumes:
- "bind-vol:/container-dir"
- "./code:/code"
```

### Bind mounting

```bash
docker run -p 5432:5432 -v ./app:/usr/lib/data/postgres:ro myimage:v0.0.1 
# -v for volume, will sync data from local dir /app to CT dir /postgres with read-only permissions
```

### Anonymous mounting

```bash
docker run -p 5432:5432 -v /var/lib/mysql/data
# will auto create dir on host name, smth like /var/lib/docker/volumes/dlfhsdofh3oh3hdahd/_data
```

### Volume mounting (used for prod servers)

```bash
docker run -p 5432:5432 -v just_dir_name_with_no_path:/var/lib/mysql/data

# where to find named volume

# Windows
C:\ProgramData\docker\volumes

# Linux
cd /var/lib/docker/volumes

# OSx
/var/lib/docker/volumes # OSx creates VM for Docker, so need to get into VM first
```

### Attach to another CT volume

```bash
docker volume create data_volume

docker run —itd —v data_volume:/some/ct/dir --name some-ct some:image

docker run —itd ——volumes-from some-ct ——name alpine2 alpine:latest
# or
docker run —itd —v data_volume:/some/ct/dir --name alpine2 alpine:latest
```

### Mount volume on running CT

```bash
# To mount a new folder to a running Docker container, you can use the docker container update command with the --mount flag. Here's an example:
docker container update --mount type=bind,source=/path/on/host,target=/path/on/container <container_name_or_id>

# In this command, type=bind specifies that the mount type is a bind mount, source=/path/on/host specifies the path of the folder on the host machine, target=/path/on/container specifies the path of the folder in the container, and <container_name_or_id> is the name or ID of the running container that you want to mount the folder to.

# Make sure to replace /path/on/host and /path/on/container with the actual paths you want to use.
```

### Copy from named volume to volume

```bash
docker volume create new_volume

docker run --rm -it -v old_volume:/from -v new_volume:/to -v $(pwd):/backup busybox sh

cp -av /from/. /to/.

docker volume rm old_volume
```

### Volume remove

```bash
docker volume rm nginx-data
```

### Read mode

```bash
docker run —v /opt/nginx:/usr/share/nginx/html:ro —d nginx:1.23.3 # read only mode for volume
```

### Default storage dir

```bash
ls /var/lib/docker
```

### Storage drivers

```bash
# Docker auto chooses best driver for OS (AUFS, ZFS, BTRFS, Dev Mapper, etc)
```

### NFS Volume

```bash
docker volume create \
--driver local \
--opt type=nfs \
--opt o=nfsvers=4,addr=nfs.example.com,rw \
--opt device=:/path/on/server \
foo

# compose
version: '3.7'
volumes:
	nfs-data:
		driver: local
		driver_opts:
		type: nfs
		o: nfsvers=4,addr=nfs.example.com,rw
		device: ":/path/to/dir"
services:
	app:
		volumes:
			- nfs-data:/data
```

### Ext4 Volume

```yml
version: '3.7'
volumes:
ext-data:
driver: local
driver_opts:
type: ext4
o: ro
device: "/dev/sdb1"

services:
app:
volumes:
- ext-data:/data
```

### Overlay Volume

```yml
version: '3.7'
volumes:
overlay-data:
driver: local
driver_opts:
type: overlay
device: overlay
o: lowerdir=${PWD}/data2:${PWD}/data1,\
upperdir=${PWD}/upper,workdir=${PWD}/workdir
services:
app:
volumes:
- overlay-data:/data
```