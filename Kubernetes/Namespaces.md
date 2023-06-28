### Check

Open a terminal and run the following command to list all the Namespaces in your cluster:

```bash
kubectl get namespaces
# or
kubectl get ns
```

**By default, all Kubernetes resources are created in the "default" Namespace if no Namespace is specified**. The "default" Namespace is created automatically when Kubernetes is installed.

### Create

Run the following command to create a new Kubernetes Namespace called "production". You can name your Namespace anything you like.

```bash
kubectl create namespace production

# or with file
kubectl create -f namespace-dev.yml

```

## Delete

```bash
kubectl delete namespace production
```

### NS link example

```bash
# Namespace is dev here
db-service.dev.s.svc.cluster.local
```

### Switch K8s to another namespace

```bash
kubectl config set-context $(kubectl config current-context) --namespace=dev

# to switch back to default
kubectl config set-context $(kubectl config current-context) --namespace=default

```

### Create Resource Quota for NS

```bash
# from file
kubectl create -f compute-quota.yaml

```