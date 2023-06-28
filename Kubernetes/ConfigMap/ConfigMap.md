### Create

```bash
# Create a ConfigMap with a single key, "app.conf"
kubectl create configmap my-app-config --from-file=app.conf

# Create a ConfigMap with a single key, "“app.conf" but another file
kubectl create configmap my-app-config --from-file=app.conf=app-prod.conf

# Create a ConfigMap with multiple keys (one per file in the config.d dir
kubectl create configmap my-app-config --from-file=config.d/

# Create a ConfigMap with two keys
kubectl create cm my-app-config \
--from-literal=foreground=red \
--from-literal=background=blue

# Create a ConfigMap from a file containing key=val pairs (traditional ENVs formatted file)
kubectl create cm my-app-config \
--from-env-file=app.conf

```

### Example (Docker Registry)

```bash
kubectl create configmap registry --from-literal=http.addr=0.0.0.0:80

k get cm registry -o yaml

curl -O  https://k8smastery.com/registry.yaml

###
apiVersion: v1
kind: Pod
metadata:
  name: registry
spec:
  containers:
  - name: registry
    image: registry
    env:
    - name: REGISTRY_HTTP_ADDR
      valueFrom:
        configMapKeyRef:
          name: registry
          key: http.addr
###

k apply -f registry.yaml

kubectl attach --namespace=shpod -ti shpod

kubectl get pod registry -o wide

IP=$(kubectl get pod registry -o json | jq -r .status.podIP)

# Confirm that the registry is available on port 80:
curl $IP/v2/_catalog



```