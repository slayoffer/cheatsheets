### LoadBalancer Service

A LoadBalancer Service is another way you can expose your application to external clients. However, it only works when you're using Kubernetes on a cloud platform that supports this Service type.

The LoadBalancer Service detects the cloud computing platform on which the cluster is running and creates an appropriate load balancer in the cloud provider’s infrastructure. The load balancer will have its own unique, publicly accessible IP address. For example, if the cluster is running on Amazon Web Services (AWS), the Service will create an Elastic Load Balancer (ELB) to distribute incoming traffic across multiple nodes in the cluster.
For example, say you have a web application running on a Kubernetes cluster and exposed using the LoadBalancer Service. External users can access the application by visiting the external IP address of the load balancer in their web browser. The load balancer will then forward their request to a node. If another user connects, their connection will be forwarded to a different node. This way, traffic gets distributed evenly, across multiple nodes, so that none gets overloaded with too many requests. Finally, the connection gets forwarded from node to Pod through a Service that sits in front of them. Each user will then get the response in their web browser. But users won't hit the same node, because of our LoadBalancer. So our worker nodes will have an easier time responding to requests.

Below, we have an example of a .yaml file describing a LoadBalancer Service:

```yaml
apiVersion: v1
kind: Service
metadata:
 name: nginx-load-balancer
spec:
 type: LoadBalancer
 selector:
   run: app-nginx
 ports:

 - port: 80
   targetPort: 80
```