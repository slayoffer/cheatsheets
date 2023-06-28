### Install

```bash
sudo snap install microk8s --classic

# give user k8s admin rights
sudo usermod -aG microk8s slayo
```

### Check

```bash
sudo microk8s kubectl get all --all-namespaces # no need to use sudo if user added to k8s group
```

### DNS module

```bash
sudo microk8s enable dns
```

### Show namespaces

```bash
microk8s kubectl get all --all-namespaces
```

### Create controller node

```bash
microk8s add-node
```

### Show nodes

```bash
microk8s kubectl get no
```

### Leave node (for worker nodes)

```bash
microk8s leave

# leave unreachable node
microk8s remove-node 10.22.254.79
```

### Status

```bash
microk8s status
```

### Stop cluster

```bash
microk8s stop
```

