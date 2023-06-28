### Deploy

```bash
kubectl apply -f https://k8smastery.com/ic-nginx-lb.yaml
```

### Describe

```bash
kubectl describe -n ingress-nginx deploy/ingress-nginx-controller
```

### Services deploy

```bash
k create deploy cheddar --image bretfisher/cheese:cheddar
k create deploy stilton --image bretfisher/cheese:stilton
k create deploy wensleydale --image bretfisher/cheese:wensleydale

k expose deployment cheddar --port=80
k expose deployment stilton --port=80
k expose deployment wensleydale --port=80
```

### Ingress resources

```bash
curl -O https://k8smastery.com/ingress.yaml

# edit IPs and run
k apply -f ingress.yaml
```

### Redirect

```bash
curl -O https://k8smastery.com/redirect.yaml

k apply -f redirect.yaml
```

