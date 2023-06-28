### ExternalName Service

An ExternalName Service in Kubernetes is useful when you have a service that is running outside your Kubernetes cluster, such as a database, and you want to access it from within your cluster.

For example, let's say you have an app in a Pod that needs to access an external database running at the domain name "db-prod.example.com". For this database, you can create an ExternalName Service named, say, "db-prod". Then, when the app wants to connect to the database, it will send a request to the local name "db-prod". Next, Kubernetes will look up the ExternalName Service for "db-prod" and see that it has an external name of "db-prod.example.com". Kubernetes will then use DNS to resolve the external name to an IP address and route the request to the appropriate external service outside the Kubernetes cluster. This process happens automatically and seamlessly. The app doesn’t need to be aware of the details of the external service.

Note: DNS lookup is like looking up a phone number in a directory. When you type in a website address or domain name, your computer sends a request to a special directory called a DNS server to find the IP address of the server hosting that website. This helps your computer connect to the website you want to visit.

We now understand what an ExternalName Service is, but what are the benefits of using one?

One of the key benefits of ExternalName Service in Kubernetes is that it helps you keep the details of your external service separate from your application. Instead of hard-coding IP addresses or domain names, you can give these services a nickname using an ExternalName Service. If the service provider moves to a new IP address or gets a new domain name, you can simply update the ExternalName Service definition to point to the new location. Kubernetes will handle the DNS resolution of the new address so that the application can continue to access the external service using the same local name. No modifications are required in the application code. This makes managing your external services much easier as you don't have to update the application code each time the external service changes.

Below, we have an example of .yaml file defining an ExternalName Service:

```yaml
apiVersion: v1
kind: Service
metadata:
 name: db-prod
spec:
 type: ExternalName
 externalName: db-prod.example.com 
```


Note that an ExternalName Service does not have a selector, as it does not map to any Pods.

In this example, we’re creating an ExternalName Service called "db-prod" that maps to a database running at the domain name "db-prod.example.com". Once this Service is created, our application can simply use the Service name "db-prod" to access this database running outside the Kubernetes cluster.

The following is an explanation of the key fields:

name: This field specifies the name of the Service object, in this case, "db-prod".

type: This field specifies the type of Service. In this case, the value is "ExternalName", indicating that this Service maps to an external service, not a service inside the Kubernetes cluster.

externalName: This field specifies the location at which that external service can be reached at, in this case, "db-prod.example.com".