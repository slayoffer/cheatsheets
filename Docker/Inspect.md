### Find CT ip address

```bash
docker inspect --format='{{.NetworkSettings.IPAddress}}' mynginx
```

### Inspect image

```bash
docker image inspect httpd

# list only specific value
docker image inspect httpd -f '{{.Os}}' # -f for filter

# for several values
docker image inspect httpd -f '{{.Architecture}} {{.Os}}'
# for nested values
docker image inspect httpd -f '{{.ContainerConfig.ExposedPorts}}'
```