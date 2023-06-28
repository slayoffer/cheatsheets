Below, we have an example of a .yaml file describing a NodePort Service object:

```yaml
apiVersion: v1
kind: Service
metadata:
 name: nginx-nodeport
spec:
 type: NodePort
 selector:
   run: app-nginx
 ports:

 - nodePort: 30001
   port: 80
   targetPort: 80
```

