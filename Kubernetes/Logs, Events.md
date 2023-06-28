### Show

```bash
k logs nginx

# tail
k logs deploy/pingpong --tail 100

# follow
k logs deploy/pingpong --tail 1 --follow

# with ENV
POD=$(k get pod -l app=rng,pod-template-hash -o name)
k logs --tail 1 --follow $POD
```

### Cluster events

```bash
k get events -w
```

### Show specific pod logs

```bash
# get pod name
k get pods

# check
k logs pingpong-6d8f458c56-b4wfm
```

### Node logs

```bash
kubectl get --raw "/api/v1/nodes/node.example/proxy/logs/?query=kubelet”

```

### Label

```bash
k logs -l app=pingpong --tail 1 -f
```

### Increase logs requests count via API

```bash
--max-log-requests

```

### Stern

```bash
k stern pingpong

# with timestamps
k stern --tail 1 --timestamps pingpong
```