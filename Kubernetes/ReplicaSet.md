### Create

```bash
kubectl create -f rs-definition.yml
```

### Get

```bash
kubectl get replicaset
# or
kubectl get rs

# check pods of rs
kubectl get pods

```

### Update

```bash
# update rs yaml file and then run cmd
kubectl replace -f rs-definition.yml

# or
kubectl scale rs new-replica-set --replicas=5

# or
kubectl scale --replicas=6 -f replicaset-definition.yml # wont update rs file

# or
kubectl scale --replicas=6 replicaset myapp-replicaset # wont update rs file

# or
kubectl scale rs --replicas=6 myapp-replicaset # wont update rs file


```

### Delete

```bash
kubectl delete replicaset myapp-replicaset

# delete pod
kubectl delete pod myapp-replicaset-pod-name

```

### Describe

```bash
kubectl describe replicaset myapp-replicaset
```

### Edit

```bash
kubectl edit replicaset myapp-replicaset

```

### Explain

```bash
kubectl explain replicaset | grep VERSION
```

### Check annotations

```bash
kubectl describe replicasets -l app=worker | grep -A3 Annotations

```