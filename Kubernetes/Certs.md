1. Obtain the `kubeadm` configuration file from the cluster and save it locally
```bash
kubectl -n kube-system get configmap kubeadm-config -o jsonpath='{.data.ClusterConfiguration}' --insecure-skip-tls-verify > kubeadm.yaml
```
1. Open the file in a local text editor and and find the `certSANs` section under `apiServer`. If this is not there, then it must be created. Here’s an example of what it looks like.
```yml
apiServer:
certSANs:
- "10.10.10.100"
- "kubernetes.default"
extraArgs:
authorization-mode: Node,RBAC
timeoutForControlPlane: 4m0s
```
1. Add the new IP address or hostname to the `certSANs` section
```yml
apiServer:
certSANs:
- "10.10.10.100"
- "kubernetes.default"
- "new-hostname-kubernetes"
- "X.X.X.X"
extraArgs:
authorization-mode: Node,RBAC
timeoutForControlPlane: 4m0s
```
1. SSH to the API server (control plane) and move the old certificates to another folder so that `kubeadm` can recreate new ones:
```bash
sudo mv /etc/kubernetes/pki/apiserver.{crt,key} ~
```
1. Use `kubeadm` to generate new apiserver certificates:
```bash
sudo kubeadm init phase certs apiserver --insecure-skip-tls-verify --config kubeadm.yaml
# or if doesnt work
sudo kubeadm init phase certs apiserver --config kubeadm.yaml
```
1. Now stop and restart the kubeapiserver container using kubeadm, docker or critools

```bash
kubectl get pods -n kube-system

kubectl delete pod kube-apiserver-k8s-master -n kube-system

kubectl get pods -n kube-system
```

7. Or run `docker ps | grep kube-apiserver | grep -v pause` to get the container ID for the container running the Kubernetes API server
   
8. Run `docker kill <containerID>` to kill the container.
   
9. The `kubelet` will automatically restart the container, which will pick up the new certificates.
   
10. If everything is working as expected, upload the kubeadm configuration for future upgrades

```bash
sudo kubeadm init phase upload-config kubeadm --config kubeadm.yaml

kubectl get nodes
```