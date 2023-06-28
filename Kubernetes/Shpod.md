### Cmds

```bash
# start
kubectl apply -f https://k8smastery.com/shpod.yaml

# attach:
kubectl attach --namespace=shpod -ti shpod

# or better exec
kubectl exec --namespace=shpod -it shpod -- /bin/sh

# 2nd window, create a new bash shell:
kubectl exec --namespace=shpod -ti shpod -- bash -l
```

### Get IP of service

```bash
IP=$(kubectl get svc httpenv -o go-template --template '{{.spec.clusterIP}}')

curl http://$IP:8888/
# or
curl -s http://$IP:8888/ | jq .HOSTNAME

```

### Delete

```bash
kubectl delete -f https://bret.run/shpod.yml
```

### Apache benchmark (for healthchecks)

```bash
# get needed svc cluster ip
k get svc

# get into shpod
kubectl attach --namespace=shpod -ti shpod

# overload svc with network traffic
ab -c 10 -n 1000 http://10.104.37.172/1
```