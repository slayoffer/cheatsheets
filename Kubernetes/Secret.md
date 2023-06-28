### Create

```bash
kubectl create secret docker-registry my-registry-key --docker-server=$CI_REGISTRY --docker-username=$GITLAB_USER --docker-password=$GITLAB_PASSWORD -n my-micro-service --dry-run=client -o yaml
```