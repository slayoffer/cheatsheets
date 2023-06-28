### Create

```bash
# run directly, not from file
kubectl run nginx --image nginx

# from file
kubectl apply -f pod.yaml # always try to apply instead of create
# or
kubectl create -f pod.yaml

# create in namespace
kubectl create -f pod-definition.yml --namespace=dev
# or
kubectl run nginx --image nginx -n dev

# with label
kubectl run redis -l tier=db --image=redis:alpine

# with command
kubectl run pingpong --image alpine --command -- ping 1.1.1.1

# with port
kubectl run custom-nginx --image=nginx --port=8080

# with port and expose - will create ClusterIp service also

kubectl run custom-nginx --image=nginx --port=8080 --expose=true

# dry run
kubectl run nginx --image nginx --dry-run

# dry run with yaml output
kubectl run nginx --image nginx --dry-run=client -o yaml

# dry run with yaml output and file creation
kubectl run nginx --image nginx --dry-run=client -o yaml > nginx.yaml

# from folder
kubectl apply -f kubernetes/

# if subfolders inside
kubectl apply -R -f kubernetes/ # -R for recursive

# add 00_prefix for files to be evaluated first
# add 01_prefix for second order, etc
```

### Inspect

```bash
# show pod details
kubectl describe pod nginx
```

### Get

```bash
kubectl get pods

# detailed view
kubectl get pods -o wide

# show with labels, -l for label, to list pods with specific label
kubectl get pods -l app=telnet-server

# with selector
kubectl get pods --selector app=App1

# show from namespace
kubectl get pods --namespace=kube-system
# or
kubectl get pods -n kube-system

# show from all NSes
kubectl get pods --all-namespaces
# or
kubectl get pods -A

```

### Delete pod

```bash
kubectl delete pod webapp
```

### Edit, Update

```bash
# bad way
kubectl edit pod redis

# right way
vim sample.yaml

# then
kubectl replace -f sample.yaml

# or better
kubectl apply -f sample.yaml

# delete and recreate
kubectl replace --force -f nginx.yaml

```

### Show K8s system pods

```bash
# for example Etcd db
kubectl get pods -n kube-system

```

### Create Yml Pod definition

```bash
# will show yml cmds for this deployment
kubectl run redis —image=redis123 —dry-run=client -o yaml

# can pipe it to yml file
kubectl run redis —image=redis123 —dry-run=client -o yaml > redis.yml

```

### Force Recreate

```bash
kubectl replace --force -f nginx.yaml
```

### Logs

```bash
# for one pod
kubectl logs pod_name

# for all pods
kubectl logs -l app=telnet-server --all-containers=true --prefix=true # prefix for pod names
```

### Scale pods

```bash
kubectl scale deployment telnet-server --replicas=3
# EMERGENCY ONLY! Better not do it in real time, as if someone pushes new deployment manifest, it will replace replicas count back to old values
```

### Check pods and services connection

```bash
kubectl get endpoints -l app=telnet-server
```

### Assign pod to specific/another node

```bash
# better to delete pod and set node in manifest file
kubectl delete po nginx
kubectl create -f nginx.yaml

# or
kubectl replace --force -f nginx.yaml


# if cant delete create a binding object yaml file and convert it to json format and then run
> curl --header "Content-Type:application/json" --request POST --data ‘{"apiVersion":"v1", "kind": “Binding” ... }
http://$SERVER/api/v1/namespaces/default/pods/$PODNAME/binding/

```

### Filter

```bash
kubectl get pods --selector env=dev --no-headers | wc -l
```

### Get pods IP

```bash
kubectl get pods -l app=littletomcat -o jsonpath='{.items[0].status.podIP}'
```