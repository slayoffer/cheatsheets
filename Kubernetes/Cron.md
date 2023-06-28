### Run

```bash
kubectl create cronjob cronjob --image alpine --schedule='@daily' -- sleep 10

```

### Get

```bash
k get cronjobs
# or
k get cj
```

### Delete

```bash
k delete cronjob cronjob
```