You can have Docker automatically rotate the logs for you. This is done with additional flags to dockerd if you are using the default [JSON logging driver](https://docs.docker.com/config/containers/logging/json-file/):

```yaml
dockerd ... --log-opt max-size=10m --log-opt max-file=3
```

You can also set this as part of your [daemon.json](https://docs.docker.com/engine/reference/commandline/dockerd/#on-linux) file instead of modifying your startup scripts:

```yaml
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3" 
  }
}
```

These options need to be configured with root access. Make sure to run a `systemctl reload docker` after changing this file to have the settings applied. This setting will then be the default for any newly created containers. Note, existing containers need to be deleted and recreated to receive the new log limits.

---

Similar log options can be passed to individual containers to override these defaults, allowing you to save more or fewer logs on individual containers. From `docker run` this looks like:

```yaml
docker run --log-driver json-file --log-opt max-size=10m --log-opt max-file=3 ...

# or with logger to check (https://github.com/sudo-bmitch/docker-loggen)
docker container run -d --log-opt max-size=10m --log-opt max-file=3 --name loggen-ex sudobmitch/loggen 150 180

docker container run -d --log-driver local --log-opt max-size=10m --log-opt max-file=3 --name loggen-ex sudobmitch/loggen 150 180
```

or in a compose file:

```yaml
version: '3.7'
services:
  app:
    image: ...
    logging:
      options:
        max-size: "10m"
        max-file: "3"
```

---

For additional space savings, you can switch from the json log driver to the "local" log driver. It takes the same max-size and max-file options, but instead of storing in json it uses a binary syntax that is faster and smaller. This allows you to store more logs in the same sized file. The daemon.json entry for that looks like:

```yaml
{
  "log-driver": "local",
  "log-opts": {"max-size": "10m", "max-file": "3"}
}
```

The downside of the local driver is external log parsers/forwarders that depended on direct access to the json logs will no longer work. So if you use a tool like filebeat to send to Elastic, or Splunk's universal forwarder, I'd avoid the "local" driver.

I've got a bit more on this in my [Tips and Tricks presentation](https://sudo-bmitch.github.io/presentations/dc2019/tips-and-tricks-of-the-captains.html#logs).

### X Defaults

```yml
version: '3.7'
x-defaults:
service: &default-svc
image: sudobmitch/loggen
logging: { options: { max-size: "10m", max-file: "3" } }
services:
cat:
<<: *default-svc
command: [ "300", "120" ]
environment: { pet: "cat" }
turtle:
<<: *default-svc
labels: { name: "gordon", levels: "all the way down" }
```