### Taint node

```bash
kubectl taint nodes node01 app=blue:NoSchedule
# NoSchedule will no put pods on node
# NoExecute setting will no put pods on node and will evict currently existing pods which dont have tolerance
# PreferNoSchedule setting will no put pods on node but its not guaranteed
```

### Untaint node

```bash
kubectl taint nodes controlplane node-role.kubernetes.io/control-plane:NoSchedule-
```

### Pod Toleration

```yaml
# add toleration to pod manifest and apply it

---
apiVersion: v1
kind: Pod
metadata:
  name: bee
spec:
  containers:
  - image: nginx
    name: bee
  tolerations:
  - key: spray
    value: mortein
    effect: NoSchedule
    operator: Equal
```

### Check node taints

```bash
kubectl describe node k8s-master | grep Taint
```