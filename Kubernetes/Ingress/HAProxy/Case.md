### Example

```bash
curl -O https://k8smastery.com/haproxy.cfg

kubectl create configmap haproxy --from-file=haproxy.cfg

kubectl get configmap haproxy -o yaml

k apply -f https://k8smastery.com/haproxy.yaml

# get pod IP
k get po haproxy -o wide

k attach --namespace shpod -it shpod

IP=$(kubectl get pod haproxy -o json | jq -r .status.podIP)

curl $IP


```