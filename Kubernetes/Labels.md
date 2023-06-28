### Add

```bash
# add label "enabled" to pods with label "rng"
k label pod -l app=rng enabled=yes

# if add labels to yaml, need to quote numbers and "yes", "no" values
```

### Change existing label

```bash
k label pod -l app=rng enabled=yes --overwrite
```

### Remove label

```bash
k label pod -l app=rng enabled-
```

### Select only pods from deployments

```bash
# "pod-template-hash" label is always created by deployment
k label pod -l app=rng,pod-template-hash enabled-
# it will not touch DaemonSet, only Deployment
```