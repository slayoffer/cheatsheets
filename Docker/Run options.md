

### Run CT with several CMDs

```bash
# In order to run multiple commands in docker, use /bin/bash -c with a semicolon ;
docker run image /bin/bash -c "cd /some/path; python a.py"

# In this case the second command (python) will be executed only if the first command (cd) returns no error or an exit status. To avoid this use && instead of ; (semi-colon)
docker run image /bin/bash -c "cd /some/path && python a.py"

# for piping use
docker run img /bin/bash -c "ls -1 | wc -l"
```





### Run command history

`docker ps --no-trunc` and `docker inspect CONTAINER` provide the entrypoint executed to start the container, along the command passed to, but that may miss some parts such as `${ANY_VAR}` because container environment variables are not printed as resolved.

To overcome that, `docker inspect CONTAINER` has an advantage because it also allow to retrieve separately env variables and their values defined in the container from the `Config.Env` property.

`docker ps` and `docker inspect` provide information about the executed entrypoint and its command. Often, that is a wrapper entrypoint script (`.sh`) and not the "real" program started by the container. To get information on that, requesting process information with `ps` or `/proc/1/cmdline` help.

### Get run cmd from the running CT

```bash
docker inspect nexusrubiuscom_nexus_1 | jq '.[0].Config.Cmd'
# or
docker inspect alpine-example
# or
docker inspect alpine-example | grep -4 -E "Cmd|Env"
# or
docker inspect --format '{{.Name}} {{.Config.Cmd}} {{ (.Config.Env) }}' alpine-example
# will show /alpine-example [sh -c ls $MY_VAR]  [MY_VAR=/var PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin]
```

### User-friendly formatting with docker ps

```bash
docker ps -a --filter name=sonarqube_sonarqube_1 --no-trunc
# or better
docker ps --filter name=sonarqube_sonarqube_1 --no-trunc --format "table{{.Names}}\t{{.CreatedAt}}\t{{.Command}}"

# make alias
alias dps='docker ps --no-trunc --format "table{{.Names}}\t{{.CreatedAt}}\t{{.Command}}"'
```



### Retrieve the started process from the container itself for running containers

```bash
# The entrypoint and command executed by docker may be helpful but in some cases, it is not enough because that is "only" a wrapper entrypoint script (.sh) that is responsible to start the real/core process.

# For example when I run a Nexus container, the command executed and shown to run the container is "sh -c ${SONATYPE_DIR}/start-nexus-repository-manager.sh".

# For PostgreSQL that is "docker-entrypoint.sh postgres".

# To get more information, we could execute on a running container docker exec CONTAINER ps aux.
# It may print other processes that may not interest us.
# To narrow to the initial process launched by the entrypoint, we could do :
docker exec CONTAINER ps -1

# I specify 1 because the process executed by the entrypoint is generally the one with the 1 id.

#Without ps, we could still find the information in /proc/1/cmdline (in most of Linux distros but not all). 
# For example:
docker exec CONTAINER cat /proc/1/cmdline | sed -e "s/\x00/ /g"; echo    

# If we have access to the docker host that started the container, another alternative to get the full command of the process executed by the entrypoint is : : execute ps -PID where PID is the local process created by the Docker daemon to run the container such as:
ps -$(docker container inspect --format '{{.State.Pid}}'  CONTAINER)
```

