### Install

```bash
# set Flannel overlay network
kubectl apply -f https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
kubectl get pods -n kube-flannel
kubectl get nodes -o wide

# if need to make changes in network config
wget https://raw.githubusercontent.com/flannel-io/flannel/master/Documentation/kube-flannel.yml
vim kube-flannel.yml
kubectl apply -f kube-flannel.yml
```