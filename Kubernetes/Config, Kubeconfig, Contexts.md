### Contexts

```bash
# check current context details
kubectl config view

# check contexts
kubectl config get-contexts

# set new context
kubectl config use-context k8s-proxmox
# or
kubectl config use k8s-pve-admin@k8s-pve

# check nodes
kubectl get nodes
```

### Use config

```bash
kubectl get node --kubeconfig ~/.kube/config
```

### Get configmaps

```bash
# get
kubectl -n kube-public get configmaps

# describe in yaml
kubectl -n kube-public get configmap cluster-info -o yaml
```

### KUBECONFIG ENV

```bash
# add new context file
export KUBECONFIG=${KUBECONFIG:-~/.kube/config}:~/.kube/test.yml
# or
export KUBECONFIG=~/.kube/config:~/.kube/cluster0:~/.kube/cluster1:~/.kube/cluster3
# or
export KUBECONFIG=~/.kube/config

```

### K3s config set

```bash
# on k3s node
sudo cat /etc/rancher/k3s/k3s.yaml

# copy contents of k3s.yaml to your local machine
~/.kube/config # Be sure to update the `server:` to your load balancer ip or hostname
```