### Create

```bash
kubectl create serviceaccount cicd—sa --namespace=my—micro—service
```

### Add permissions

```bash
# create role yaml file with permissions and apply (create) it inside k8s
kubectl apply -f cicd—role.yaml

# add role to service account
kubectl create rolebinding cicd-rb --role=cicd-role --serviceaccount=namespace_name:cicd-sa --namespace=namespace_name

# create kubeconfig file for service account
cp ~/.kube/config ~/.kube/sa_config

# update user and contexts section in file

# for that list service accounts
kubectl get serviceaccount —n my—micro—service

# get service account yaml and check secrets section
kubectl get serviceaccount cicd—sa —n my—micro—service —o yaml

# show secret
kubectl get secret cicd—sa—token—4fkgn —n my—micro—service —o yaml

# decode secret and add it to ~/.kube/sa_config user token
echo token_content | base64 -D

# user name can be defined by yourself, may be different from SA name
# set same user name in context section
```

### Check

```bash
kubectl get namespace

kubectl get pod -n my-micro-service

kubectl get service —n my—micro—service
```

### Edit

```bash
# just edit applied (bound) role yaml file as you need and thats it
```