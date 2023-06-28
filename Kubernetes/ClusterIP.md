### ClusterIP Service

ClusterIP Services are used for communication between Pods within the same Kubernetes cluster.

The Service gets exposed on a static IP that's unique within the cluster. When you make a request to that IP, the Service takes care of redirecting traffic to one of the Pods it's associated with. And if there's more than one Pod, the Service will automatically balance the traffic, so no single Pod gets bogged down by too many requests.

Note that ClusterIP Services are meant for Pod-to-Pod communication only. They aren't accessible from outside the cluster.

Here is an example of a .yaml file describing a ClusterIP Service object:

```yaml
apiVersion: v1
kind: Service
metadata:
 name: nginx-clusterip
spec:
 type: ClusterIP
 selector:
   run: app-nginx
 ports:

 - port: 80
   protocol: TCP
```
