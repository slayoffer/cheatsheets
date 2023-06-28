### Backup a container

Backup docker data from inside container volumes and package it in a tarball archive. 

```bash
# backup portainer inside /data folder via helper ct busybox
docker run --rm --volumes-from portainer -v $(pwd):/backup busybox tar cvfz /backup/portainer-backup.tar /data

# then move portainer-backup.tar via scp to another server and start portainer CT there with same named volume with name "data" attached to /data folder inside CT

# then untar backup in the portainer CT on new server via helper container busymox
docker run --rm --volumes-from portainer -v $(pwd):/backup busybox sh -c "cd /data && tar xvf /backup/portainer-backup.tar --strip 1"

# if was copying folder inside folder, for example
docker run --rm --volumes-from tuz-ar-app-backend-prod -v $(pwd):/backup busybox tar cvfz /backup/tuz-backup.tar /app/Files

# then untar this way
docker run --rm --volumes-from tuz-ar-app-backend-prod -v $(pwd):/backup busybox sh -c "cd /app && tar xvf /backup/tuz-backup.tar --strip 1"
```

An automated backup can be done also by this [Ansible playbook](https://github.com/thedatabaseme/docker_backup). The output is also a (compressed) tar. The playbook can also manage the backup retention. So older backups will get deleted automatically.

To also create and backup the container configuration itself, you can use `docker-replay`for that. If you lose the entire container, you can recreate it with the export from `docker-replay`. A more detailed tutorial on how to use docker-replay can be found [here](https://thedatabaseme.de/2022/03/18/shorty-generate-docker-run-commands-using-docker-replay/).

### SCP copy volume to another server

```bash
sudo su

scp -r -i /home/administrator/.ssh/rubius_rsa_key /var/lib/docker/volumes/sonarqube_conf localadmin@server-docker-2.rubius.rubius.com:/home/localadmin/volumes/

cp -r /home/localadmin/volumes/sonarqube_data/ /var/lib/docker/volumes/
```

### Copy named volumes

```bash
docker run -it --rm \
    -v tuz-ar-app-backend-prod_tuz-ar-backend-stage:/source \
    -v tuz-ar-backend-stage:/target \
    alpine sh

cp -a /source/. /target
```