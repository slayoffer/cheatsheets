### Create

```bash
kubectl create -f service.yml

# The one below will not use the pods labels as selectors, instead it will assume selectors as **app=redis. You cannot pass in selectors as an option. So it does not work very well if your pod has a different label set. So generate the file and modify the selectors before creating the service
kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml
```

### Expose pod

```bash
# it will use existing pod redis labels and expose its port and create yaml code but without svc creation
kubectl expose pod redis --port=6379 --name redis-service --dry-run=client -o yaml
# or without yaml
kubectl expose pod redis --port=6379 --name redis-service

# This will automatically use the pod’s labels as selectors, but you cannot specify the node port. You have to generate a definition file and then add the node port in manually before creating the service with the pod.
kubectl expose pod nginx --type=NodePort --port=80 --name=nginx-service --dry-run=client -o yaml

# or
kubectl create service nodeport nginx --tcp=80:80 --node-port=30080 --dry-run=client -o yaml
# this one can set port, but cant set labels from pod
```

### Get

```bash
kubectl get services
# or
kubectl get svc

# check service
curl http://10.0.0.14:30008/
curl http://10.0.0.15:30008/
curl http://10.0.0.16:30008/

# show with labels
kubectl get services -l app=telnet-server

# show with namespace
k get svc -n marketing

```

### Delete

```bash
# to delete all
kubectl delete svc --all
```

### Inspect

```bash
k describe svc kubernetes
```