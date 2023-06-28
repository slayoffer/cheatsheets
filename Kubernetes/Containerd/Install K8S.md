### Preliminary steps

```bash
sudo apt install containerd

sudo mkdir /etc/containerd
containerd config default | sudo tee /etc/container/config.toml
sudo vim /etc/containerd/config.toml
# change
SystemdCgroup = true
# need to disable swap for k8s
sudo swapoff -a

free -m # swap should be zero

# comment out swap line in fstab
sudo vim /etc/fstab

# enable bridge mode
sudo vim /etc/sysctl.conf
# uncomment
net.ipv4.ip forward=1

# create config and enable kernel module
sudo vim /etc/modules-load.d/k8s.conf
# add
br_netfilter

sudo reboot
```

### Install

```bash
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt update

sudo apt install kubeadm kubectl kubelet

# initialize k8s controller node
sudo kubeadm init --control-plane-endpoint=172.16.250.216 --node-name controller --pod-network-cidr=10.244.0.0/16 # dont change CIDR IP! Can change node-name and must change control-endpoint ip

# if success then should print "join"
# do three cmds which k8s will ask you to do (create dir, copy config and change ownership)
# check
kubectl get pods --all-namespaces
```

### Deploy overlay network (if not using K8S PaaS)

```bash
kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
```

### List nodes

```bash
kubectl get nodes
```

### Join node

```bash
# example cmd
sudo kubeadm join 192.168.1.38:6443 --token asdjnsajdn --discovery-token-ca-cart-hash sha256fdjnfjf

# check
kubectl get nodes
```

### Regenerate join token

```bash
kubeadm token create --print-join-command
```

