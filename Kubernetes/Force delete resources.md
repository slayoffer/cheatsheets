Yes, sometimes namespaces may get stuck in the `Terminating` state due to finalizers not being removed properly, among other potential issues. Here's a way to force delete a namespace.

First, you need to dump the namespace content to a JSON file. Let's call it `shpod.json`.

```bash
kubectl get namespace shpod -o json > shpod.json
```

Next, you need to remove the `kubernetes` finalizer from the JSON file you've just created. Open the file in any text editor and find the `spec.finalizers` section. It should look something like this:

```json
"spec": {
    "finalizers": [
        "kubernetes"
    ]
},
```

Remove the `"kubernetes"` line so that it becomes:

```json
"spec": {
    "finalizers": []
},
```

Save and close the file.

Now, use the `curl` command to make a direct API call to the Kubernetes API server to replace the namespace definition without the finalizer:

```bash
curl -k -H "Content-Type: application/json" -X PUT --data-binary @shpod.json http://localhost:8080/api/v1/namespaces/shpod/finalize
```

Please note: 

1. This command assumes that your Kubernetes API server is accessible at `localhost:8080`. If it's not, please change the URL accordingly. 
   
2. The `-k` option is used to allow connections to SSL sites without certificates. This should only be used for debugging.

After running the `curl` command, the namespace should be deleted immediately. 

**Be careful with this operation as it's a force deletion and could lead to unforeseen side effects.** Always try to understand why a namespace is stuck in `Terminating` state before force deleting it.