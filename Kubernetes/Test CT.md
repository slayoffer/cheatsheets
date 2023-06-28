### Create and curl other svc

```bash
kubectl run --restart=Never --image=alpine -ti --rm testcontainer

apk add curl

curl nginx
```