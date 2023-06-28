### Check

```bash
kubectl get pods --namespace kube-system
```

### Run on specific node

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  -  image: nginx
     name: nginx
  nodeName: node01
```

### Manual scheddule after pod is created

```bash
# possible only via curl request with json
```

