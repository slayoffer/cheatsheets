### Enable loadbalancer (for external IP)

```bash
minikube tunnel # needs to be run always, terminal session cant be closed
```

### Show namespace service

```bash
minikube -n monitoring service grafana-service # -n for namespace, monitoring is namespace, grafana is service
```

