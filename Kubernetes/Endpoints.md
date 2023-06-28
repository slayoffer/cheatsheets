### Get

```bash
k get endpoints -o yaml

# enpoints should match with pods ips
k get pods -l app=httpenv -o wide
```