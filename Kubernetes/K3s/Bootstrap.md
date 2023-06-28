### Server init

```bash
# generate random token beforehand
curl -sfL https://get.k3s.io | sh -s - server \
--token=32MprR2D4aYnLfUe \
--node-taint CriticalAddonsOnly=true:NoExecute \
--tls-san load_balancer_ip_or_hostname \
--cluster-init # for DB

# add extra master node
curl -sfL https://get.k3s.io | sh -s - server --token=SECRET --node-taint CriticalAddonsOnly=true:NoExecute --tls-san load_balancer_ip_or_hostname
```

### Worker join

```bash
sudo chmod 644 /etc/rancher/k3s/k3s.yaml

# check token
sudo cat /var/lib/rancher/k3s/server/node-token


# join
curl -sfL https://get.k3s.io | sh -s - server \
--token=32MprR2D4aYnLfUe \
--server https://10.0.0.11:6443

# or
curl -sfL https://get.k3s.io | K3S_URL=https://load_balancer_ip_or_hostname:6443 K3S_TOKEN=mynodetoken sh -
```

### Check

```bash
sudo k3s kubectl get nodes
```

### Switch context on K8S server to K3S cluster

```bash
# on k3s node
sudo cat /etc/rancher/k3s/k3s.yaml

# copy contents of k3s.yaml to your local machine
~/.kube/config # Be sure to update the `server:` to your load balancer ip or hostname

# check current context
kubectl config view

# add new context file
export KUBECONFIG=${KUBECONFIG:-~/.kube/config}:~/.kube/test.yml
# or
export KUBECONFIG=~/.kube/config:~/.kube/cluster0:~/.kube/cluster1:~/.kube/cluster3
# or
export KUBECONFIG=~/.kube/k8s-proxmox.yml

# check contexts
kubectl config get-contexts

# set new context
kubectl config use-context k8s-proxmox
# or
kubectl config use k8s-proxmox

# check nodes
kubectl get nodes

```

## Database Backups

etcd (k3s DB) snapshots are stored in `/var/lib/rancher/k3s/server/db/snapshots`.