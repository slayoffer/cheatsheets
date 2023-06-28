### Status

```bash
k set image deploy worker worker=dockercoins/worker:v0.3

# check status
k rollout status deploy worker
```

### Rollback

```bash
# works only for 1 step before! if do it again it will flip to last one used, not 2 steps before
k rollout undo deploy worker
```

### History

```bash
kubectl rollout history deployment worker

```

### Rollback to other revision

```bash
k rollout undo deploy worker --to-revision 1
```