### Example

```bash
curl -O https://k8smastery.com/just-a-pod.yaml

k apply -f just-a-pod.yaml

# change file
vim just-a-pod.yaml

# check diff
k diff -f just-a-pod.yaml

k delete -f just-a-pod.yaml
```