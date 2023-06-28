### Live restore (keep CTs alive)

```bash
# to keep CTs running even if Docker deamon goes down
echo '{"live-restore": true}' >> /etc/docker/daemon.json

#or
vim /etc/docker/daemon.json

# add
{
"debug": true,
"hosts": ["tcp://192.168.1.10:2376"],
# add this line
"live-restore": true
}
```

### To test Live Restore on some CT get its ID

Save the ID of the container you want to edit for easier access to the files.

```sh
export CONTAINER_ID=`docker inspect --format="{{.Id}}" <YOUR CONTAINER NAME>`
```

### Edit Container Configuration

Edit the configuration file, go to the "Env" section, and add your key.

```sh
sudo vim /var/lib/docker/containers/$CONTAINER_ID/config.v2.json
```

My file looks like this:

```sh
...,"Env":["TEST=1",...
```

### Stop and Start Docker

I found that restarting Docker didn't work, I had to stop and then start Docker with two separate commands.

```sh
sudo systemctl stop docker
sudo systemctl start docker
```

Because of `live-restore`, your containers should stay up.

### Verify That It Worked

```sh
docker exec <YOUR CONTAINER NAME> bash -c 'echo $TEST'
```

Single quotes are important here.

You can also verify that the uptime of your container hasn't changed:

```sh
docker ps
```