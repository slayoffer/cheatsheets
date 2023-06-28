### Help

```bash
# check version
kubectl version

# explain
kubectl explain deployment.metadata.labels
```

### Check cluster

```bash
kubectl cluster-info

# show nodes
kubectl get nodes

# show pods and services together
kubectl get pods,svc

# show nodes with OS type, IPs, Kernel, etc
kubectl get nodes -o wide

# show all cluster objects
kubectl get all

# show yaml
k get no -o yaml

# with labels
kubectl get all --selector env=prod
kubectl get all --selector env=prod,bu=finance,tier=frontend
```

### List cron jobs

```bash
kubectl get cronjobs.batch -l app=bb-warrior
```

### With JQ

```bash
kubectl get nodes -o json |
jq ".items[] | {name:.metadata.name} + .status.capacity"

```

### Label node

```bash
k label no node01 label=value

# add to pod yaml
spec:
    nodeSelector:
        label: value
```

