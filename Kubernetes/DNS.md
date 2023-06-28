### DNS for Services and Pods

Kubernetes creates DNS records for Services and Pods. You can contact Services with consistent DNS names instead of IP addresses.

```bash
your-service.your-namespace.svc.cluster.local

# curl
curl littletomcat-service.shpod.svc.cluster.local:8080
```

Any Pods exposed by a Service have the following DNS resolution available:

```
your-pod.your-service.your-namespace.svc.cluster.local
```

---
### Custom DNS Settings

### Edit coredns config map

Add entry to the `Corefile: |` section of the `configmap/coredns` in section **kube-system**.

```yml
.:53 {
  # ...
}
import /etc/coredns/custom/*.server
```

### Add new config map

Example for local DNS server using the **clcreative.home** zone.

```yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns-custom
  namespace: kube-system
data:
  clcreative.server: |
    clcreative.home:53 {
      forward . 10.20.0.10
    }
```