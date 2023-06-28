### Client

```bash
k apply -f web.yaml --dry-run=client --validate=false -o yaml
```

### Server

```bash
k apply -f web.yaml --dry-run=server --validate=false -o yaml
```