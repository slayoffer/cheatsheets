## Creating a DaemonSet

A DaemonSets is created by submitting a DaemonSet YAML configuration file to the Kubernetes API server. The example below demonstrates how to create a fluentd logging agent on each node in a specific cluster.

We start by creating a file, "fluentd.yaml" describing the DaemonSet.

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
 name: fluentd
spec:
 selector:
   matchLabels:
     app: fluentd
 template:
   metadata:
     labels:
       app: fluentd
   spec:
     containers:
     - name: fluentd
       image: fluent/fluentd
       volumeMounts:
       - name: varlog
         mountPath: /var/log
       - name: varlibdockercontainers
         mountPath: /var/lib/docker/containers
         readOnly: true
     terminationGracePeriodSeconds: 30
     volumes:
     - name: varlog
       hostPath:
         path: /var/log
     - name: varlibdockercontainers
       hostPath:
         path: /var/lib/docker/containers
```

A DaemonSet configuration file needs to have the following fields:

- **apiVersion:** The "apiVersion" field refers to the version of the Kubernetes API you're using to create this object.
- **kind:** The "kind" field specifies the type of object being created (a DaemonSet in this case).
- **metadata:** The "metadata" field provides information about the object being created, including the name of the object.
- **spec:** The "spec" field is where you provide the actual details of the DaemonSet, including the Pod template (.spec.template). The Pod template is a template of the Pod that the DaemonSet will be creating replicas of. In addition to the required fields for a normal Pod, a Pod template in a DaemonSet must also specify appropriate labels. These labels are used to identify the Pods that the DaemonSet is responsible for. You also need to specify a Pod selector in the DaemonSet's ".spec.selector" field. This selector is used to match the labels of the Pod template so that the DaemonSet knows which Pods it should be managing.

Now that you know how to write a valid DaemonSet configuration, you can use the "kubectl apply" command to submit the DaemonSet to the Kubernetes API:

```bash
kubectl apply -f fluentd.yaml
```

After the "fluentd" DaemonSet has been successfully submitted to the Kubernetes API, you can use the "kubectl describe" command to check its current state:

```bash
kubectl describe daemonset fluentd
```

Note that the screenshot above is a shortened version of the complete output.

The output shows that a "fluentd" Pod was successfully deployed on all three nodes of our cluster. You can confirm this by using the "kubectl get pods" command with the "-o" flag to display the nodes that each "fluentd" Pod was assigned to.

```bash
kubectl get pods -l app=fluentd -o wide
```

## Limiting DaemonSets to Specific Nodes

DaemonSets are most commonly used to run a Pod across every node in a Kubernetes cluster. However, there may be cases where you want to run a Pod on only a subset of nodes.

For example, if you have a workload that requires fast storage, you would want to deploy that workload only to the nodes that have fast storage available. In cases like these, you can use node labels to tag specific nodes that meet the requirements of the workload.

### Add Labels to Nodes

You can add the desired set of labels to a subset of nodes using the "kubectl label" command. The following command adds the label "ssd=true" to the node daemonset-demo-m03:

```bash
kubectl label nodes daemonset-demo-m03 ssd="true"
```

Now you can filter the node that has the "ssd" label set to "true" using the "kubectl get nodes" command with the "–selector" flag.

```bash
kubectl get nodes --selector ssd="true"
```

### Node Selectors

Node selectors are a way to control which nodes in a Kubernetes cluster a Pod can be scheduled on. This allows you to specify certain requirements for the node that the Pod will run on, such as specific hardware or storage resources.

Let's delete the DaemonSet created in the previous section and create one with node selectors.

```bash
kubectl delete daemonset fluentd
```

The  previous "fluentd" DaemonSet is now deleted. Next, replace the contents of the "fluentd.yaml" file with the code snippet below:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
 name: fluentd
spec:
 selector:
   matchLabels:
     app: fluentd
 template:
   metadata:
     labels:
       app: fluentd
   spec:
     nodeSelector:
       ssd: "true"
     containers:
     - name: fluentd
       image: fluent/fluentd
       volumeMounts:
       - name: varlog
         mountPath: /var/log
       - name: varlibdockercontainers
         mountPath: /var/lib/docker/containers
         readOnly: true
     terminationGracePeriodSeconds: 30
     volumes:
     - name: varlog
       hostPath:
         path: /var/log
     - name: varlibdockercontainers
       hostPath:
         path: /var/lib/docker/containers
```

This DaemonSet is configured to only run on nodes with the label ssd="true" by defining the "nodeSelector" field in the Pod spec.

Run the following command to submit the DaemonSet to the Kubernetes API:

```bash
kubectl apply -f fluentd.yaml
```

Since there is only one node (daemonset-demo-m03) with ssd="true" label, the "fluentd" Pod will run only on that node:

```bash
kubectl get pods -l app=fluentd -o wide
```

## Updating a DaemonSet

When you want to update a DaemonSet, you can use the RollingUpdate strategy, which allows you to incrementally update the Pod instances with new ones.

Replace the contents of your "fluentd.yaml" file with the following code snippet:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
 name: fluentd
spec:
 selector:
   matchLabels:
     app: fluentd
 updateStrategy:
   type: RollingUpdate
   rollingUpdate:
     maxUnavailable: 1
 template:
   metadata:
     labels:
       app: fluentd
   spec:
     containers:
     - name: fluentd
       image: fluent/fluentd:v1.2
       volumeMounts:
       - name: varlog
         mountPath: /var/log
       - name: varlibdockercontainers
         mountPath: /var/lib/docker/containers
         readOnly: true
     terminationGracePeriodSeconds: 30
     volumes:
     - name: varlog
       hostPath:
         path: /var/log
     - name: varlibdockercontainers
       hostPath:
         path: /var/lib/docker/containers
```

You can configure the update strategy using the "spec.updateStrategy.type" field. This field should be set to "RollingUpdate" to enable the rolling update. Any changes you make to the "spec.template" field (or subfields) of the DaemonSet will trigger a rolling update.

The "spec.updateStrategy.rollingUpdate.maxUnavailable" parameter tells Kubernetes how many Pods can be updated at the same time during the rolling update. For example, if you set it to "1", only one Pod will be updated at a time, and all other Pods will continue running normally.

To see the rolling update in action, run the following command:

```bash
kubectl apply -f fluentd.yaml
```

Once the rolling update has started, you can use the "kubectl rollout" commands to see the current status of the DaemonSet rollout. To see the current rollout status of the "fluentd" DaemonSet, run the following command:

```bash
kubectl rollout status ds/fluentd
```

## Deleting a DaemonSet

You can delete a DaemonSet using the "kubectl delete" command. Note that deleting a DaemonSet will also delete all the Pods being managed by that DaemonSet.

```bash
kubectl delete -f fluentd.yaml
# or
k delete ds/rng
```

## FAQ

### Is DaemonSet a Deployment?

No, a DaemonSet is not a Deployment. Though both are Kubernetes objects used to manage the desired state in a Kubernetes cluster, they serve different purposes.