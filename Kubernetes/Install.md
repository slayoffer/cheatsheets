### Bootstrap

```bash
# master
sudo hostnamectl set-hostname k8s-master.devcraft.app

# worker
sudo hostnamectl set-hostname k8s-worker-3.devcraft.app

# do it on all
echo "10.0.0.14 k8s-master.devcraft.app k8s-master" | sudo tee -a /etc/hosts && echo "10.0.0.15 k8s-worker-1.devcraft.app k8s-worker-1" | sudo tee -a /etc/hosts && echo "10.0.0.16 k8s-worker-2.devcraft.app k8s-worker-2" | sudo tee -a /etc/hosts

# or if use cloud template
echo "10.0.0.14 k8s-master.devcraft.app k8s-master" | sudo tee -a /etc/cloud/templates/hosts.debian.tmpl && echo "10.0.0.15 k8s-worker-1.devcraft.app k8s-worker-1" | sudo tee -a /etc/cloud/templates/hosts.debian.tmpl && echo "10.0.0.16 k8s-worker-2.devcraft.app k8s-worker-2" | sudo tee -a /etc/cloud/templates/hosts.debian.tmpl

# or edit /etc/cloud/cloud.cfg and add
manage_etc_hosts: False

# all
sudo systemctl restart systemd-hostnamed

# turn off swap
exec bash
sudo swapoff -a
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
sudo mount -a
free -h

# add kernel modules
sudo tee /etc/modules-load.d/containerd.conf<<EOF
overlay
br_netfilter
EOF

# Enable kernel modules
sudo modprobe overlay
sudo modprobe br_netfilter

# Add settings to sysctl
sudo tee /etc/sysctl.d/kubernetes.conf<<EOF
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOF

# Reload sysctl
sudo sysctl --system

# add Docker
sudo apt update

sudo apt install \
    ca-certificates \
    curl \
    gnupg
    
sudo install -m 0755 -d /etc/apt/keyrings

sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/docker.gpg

sudo chmod a+r /etc/apt/trusted.gpg.d/docker.gpg

# add Docker repo
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/trusted.gpg.d/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update

sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo mkdir /etc/containerd

containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1
# or with stdout
containerd config default | sudo tee /etc/containerd/config.toml

sudo systemctl status containerd
sudo systemctl enable containerd

# change systemcgroup to true in config
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml

# add k8s gpg
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

# add k8s repo (might need to change repo to jammy from xenial)
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

# update registry
sudo apt update

# install k8s
sudo apt install kubelet kubeadm kubectl
# hold k8s
sudo apt-mark hold kubelet kubeadm kubectl

# all set for running k8s!
_______________________________________________

# run on master node
sudo kubeadm init --control-plane-endpoint=k8s-master.devcraft.app --node-name k8s-master --pod-network-cidr=10.244.0.0/16
# or with ip
sudo kubeadm init --control-plane-endpoint=192.168.1.38 --node-name k8s-master --pod-network-cidr=10.244.0.0/16

# copy k8s config to user folder on master node
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# set Calico CNI, run first config
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/tigera-operator.yaml

# download second config
curl https://raw.githubusercontent.com/projectcalico/calico/v3.25.1/manifests/custom-resources.yaml -O

# change CIDR to 10.244.0.0/16
vim custom-resources.yaml

# run second config
kubectl create -f custom-resources.yaml

# check
watch kubectl get pods -n calico-system

# for join token run
kubeadm token create --print-join-command

# run on workers
sudo kubeadm join k8s-master.devcraft.app:6443 --token y9frkx.xvwfli9wukfo7o2o \
    --discovery-token-ca-cert-hash sha256:8ef0ba14f99f04b49b0ae1f05319779b98a501a5ba6a30f50bbcb99a1273643d

# run on master
kubectl cluster-info
kubectl get nodes
kubectl describe node k8s-worker-1

# or get nodes with namespaces
kubectl get pods --all-namespaces

# run test container to check everything
kubectl apply -f https://k8s.io/examples/pods/commands.yaml
# check CT IP
kubectl get pods -o wide
```

### Join worker node

```bash
kubeadm join k8s-master.devcraft.app:6443 \
  --token sr4l2l.2kvot0pfalh5o4ik \
  --discovery-token-ca-cert-hash sha256:c692fb047e15883b575bd6710779dc2c5af8073f7cab460abd181fd3ddb29a18
  
# then run on master node to check connection
kubectl get nodes
```

### Regenerate join token

```bash
kubeadm token create --print-join-command
```

### Check with nginx CT

```bash
vim nginx-pod.yml
# add
apiVersion: v1
kind: Pod
metadata:
  name: nginx-example
  labels:
    app: nginx
spec:
  containers:
    - name: nginx
      image: linuxserver/nginx
      ports:
        - containerPort: 80
          name: "nginx-http"
# apply
kubectl apply -f nginx-pod.yml

# get CT ip
kubectl get pods -o wide
# check
curl 10.244.1.2
```

### Expose ports for public access

```bash
vim service-nodeport.yml
# add (can use outside ports 30000-32767)
apiVersion: v1
kind: Service
metadata:
  name: nginx-example
spec:
  type: NodePort
  ports:
    - name: http
      port: 80
      nodePort: 30080
      targetPort: nginx-http
  selector:
    app: nginx
# apply
kubectl apply -f service-nodeport.yml

# check
kubectl get service
```

### Delete pod

```bash
kubectl delete pod command-demo
```

### Delete service

```bash
kubectl delete service nginx-example
```



### To reset nodes in case of any issues

```bash
sudo swapoff -a    # will turn off the swap 
sudo kubeadm reset
sudo systemctl daemon-reload
sudo systemctl restart kubelet
# not recommended
sudo iptables -F && sudo iptables -t nat -F && sudo iptables -t mangle -F && sudo iptables -X  # will reset iptables

# can also try to reset hostnames
sudo hostnamectl set-hostname masterdefaultnodename
sudo systemctl daemon-reload
sudo hostnamectl set-hostname c23996dd402c.mylabserver.com
sudo systemctl daemon-reload
kubectl get nodes
```

### To ignore errors

```bash
# for swap
kubeadm init --ignore-preflight-errors Swap
```

