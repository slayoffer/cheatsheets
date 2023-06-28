### Check connection

```bash
telnet <DOCKER_HOST_IP> 2375

nc -vz <DOCKER_HOST_IP> 2375

curl http://<DOCKER_HOST_IP>:2375/version
```

