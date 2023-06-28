### Expose deployment

```bash
k expose deploy httpenv --port 8888
# or
kubectl create service clusterip httpenv --tcp=8080

# or expose existing deploy
kubectl expose deployment littletomcat --type=ClusterIP --name=littletomcat-service --port 8080
```

### External expose

```bash
# for frontend service for example
k expose deploy webui --type NodePort --port 80
# or
kubectl create service nodeport webui --tcp=80

# check port 30000+ and go to pod_ip:port or localhost:port
k get svc
```

