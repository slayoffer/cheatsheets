### Help

```bash
kubectl create deployment --help
```

### Create

```bash
kubectl create deployment nginx --image=nginx

# or
kubectl create deployment httpd-frontend —-image=httpd:2.4-alpine —-replicas=3

# or with namespace
kubectl create namespace development
kubectl create deployment busybox-dev --image=busybox -n=development

# with command
kubectl create deployment pingpong --image alpine -- ping 1.1.1.1


# with file
kubectl create -f deployment-definition.yml

# with dry run
kubectl create deployment --image=nginx nginx --replicas=4 --dry-run=client -o yaml > nginx-deployment.yaml

# with dry run and file creation
kubectl create deployment --image=nginx nginx --dry-run=client -o yaml > nginx-deployment.yaml

# with changes tracking switched on
kubectl create -f deployment-definition.yml --record

# check
watch kubectl get pods -n calico-system

```

### Edit

```bash
kubectl edit deployment myapp-deployment --record

```

### Get deployments

```bash
kubectl get deployments

# get in namespace
kubectl get deployments -n=development

# get all
kubectl get all

```

### Describe

```bash
kubectl describe deployment

# or with deployment name
kubectl describe deployment myapp

```

### Delete

```bash
# delete using file
kubectl delete -f custom-resources.yaml

# simple delete
kubectl delete deployment myapache

# to delete all
kubectl delete deployments --all

# to delete separate
kubectl delete deployment myapache mytomcat

# delete in namespace
kubectl delete deployment alpine-dev -n=development

# delete all in all namespaces
kubectl delete deployment --all --all-namespaces

```

### Update deployment with new settings

```bash
# change file contents and apply it
kubectl apply -f deployment-definition.yml

# or
kubectl set image deployment/myapp-deployment \
ct_name=docker-registry/nginx:1.9.1

```

### Rollout status

```bash
kubectl rollout status deployment/myapp-deployment

```

### Rollback all changes

```bash
# rollback to previous version of deployment
kubectl rollout undo deployment/myapp-deployment

```

### Rollout history

```bash
kubectl rollout history deployment/myapp-deployment

```

### Scale

```bash
kubectl scale deployment voting-app-deploy --replicas=3
```

### Restart deployment

```bash
kubectl -n monitoring rollout restart deployment alertmanager # restart alertmanager deployment at monitoring namespace
```