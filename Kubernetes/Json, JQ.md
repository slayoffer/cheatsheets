### Example

```bash
# get deployments with metadate
kubectl get deploy -o json | jq ".items[] | {name:.metadata.name} + .spec.strategy.rollingUpdate"
```