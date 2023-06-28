### Get latest logs

```bash
docker logs --tail 10 my_container
# or
docker logs -f my_container
```

## Discovering the Log File’s Path

First find the path to your target container’s log file. You can retrieve the log file path for a container called `my-app` by running the following command:

```bash
docker inspect --format='{{.LogPath}}' visius-devops
```

## Clearing the Log File

You can clear out the contents of the log without deleting it by echoing an empty string into its content. The file will be owned by `root` so you’ll need to perform this operation in a root shell. The following command will empty the log file for you:

First the bad answer. From [this question](https://serverfault.com/q/637996/351549) there's a one-liner that you can run:

```yaml
echo "" > $(docker inspect --format='{{.LogPath}}' <container_name_or_id>)
```

instead of echo, there's the simpler:

```yaml
: > $(docker inspect --format='{{.LogPath}}' <container_name_or_id>)
```

or there's the truncate command:

```yaml
truncate -s 0 $(docker inspect --format='{{.LogPath}}' <container_name_or_id>)
```

Shell interpolation is being used to dynamically retrieve the log file path for the `my-app` container. You could manually substitute in the path retrieved earlier instead.

```bash
sudo sh -c 'echo "" > $(docker inspect --format="{{.LogPath}}" visius-devops)'
```

---

I'm not a big fan of either of those since they modify Docker's files directly. The external log deletion could happen while docker is writing json formatted data to the file, resulting in a partial line, and breaking the ability to read any logs from the `docker logs` cli. For an example of that happening, see [this comment on duketwo's answer](https://stackoverflow.com/questions/42510002/how-to-clear-the-logs-properly-for-a-docker-container/42510314?noredirect=1#comment98713207_43570083):

> after emptying the logfile, I get this error: `error from daemon in stream: Error grabbing logs: invalid character '\x00' looking for beginning of value`


