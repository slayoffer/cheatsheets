### Kubectl cp Syntax

Here is the syntax of the `kubectl cp` command in a simplified form.

```
kubectl cp <source> <destination>
```

Here, `<source>` and `<destination>` are the paths to the source and destination locations of the file we want to copy. Don’t worry if this seems a bit unclear right now. We’ll go through examples in the upcoming sections.

### Copy Files From Pod to Local System

We have a `nginx` web server running inside a container. Let’s copy the `index.html` file (which `nginx` serves by default) inside the `/usr/share/nginx/html` directory to our local system. Run the following command:

```
kubectl cp mynginx-ff886775c:/usr/share/nginx/html/index.html ~/desktop/index.html
```

In summary, the command above copies the `index.html` file from the container running in the Pod `mynginx-ff886775c` to the local machine's `desktop` directory.

Running the command above _won’t_ give you any output

### Copy Files From Local System to Pod

```
kubectl cp ~/desktop/index.html mynginx-ff886775c:/usr/share/nginx/html
```

To verify whether the copy was successful or not, we can check the contents of the `index.html` file inside the `nginx` container. Let's follow the steps below.

First, run the following command to get a shell to the container inside the Pod (replace `mynginx-ff886775c-mzjz2` with the name of your Pod):

```
kubectl exec -it mynginx-ff886775c-mzjz2 – /bin/bash
```

This command will open a bash shell prompt (root@mynginx-ff886775c-mzjz2: /#), as shown below:

```
cat /usr/share/nginx/html/index.html
```

This will display the contents of the file in the shell. You should see the modified welcome message, as highlighted below:

### How to copy files from a Pod in a specific Namespace to the local system

To copy files from a Pod in a specific [Namespace](https://kodekloud.com/blog/r/81cf01ea?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab) to the local system, you can **specify the Namespace name before the Pod name**. For instance, if the `mynginx` Deployment was created inside a Namespace named `demo`, to copy the file from the Pod in the `demo` Namespace to the local system or vice versa, you just need to add the Namespace name before the Pod name. Here's an example:

```
kubectl cp demo/mynginx-ff886775c-mzjz2:/usr/share/nginx/html/index.html ~/desktop/index.html
```

This command will copy the `index.html` file from the `mynginx-ff886775c-mzjz2` Pod in the `demo` Namespace to the `desktop` directory on the local system.

### How to copy files from a specific container inside a multi-container Pod to local system

To copy files from a specific container inside a multi-container Pod, you can **use the `kubectl cp` command with the `-c` flag** to specify the container's name. For instance, suppose we have a Pod named `my-pod` running two containers named `nginx` and `tomcat`. And we want to copy the `index.html` file from the `nginx` container to our local system. In that case, we can use the following command:

```
kubectl cp my-pod:/usr/share/nginx/html/index.html ~/desktop/index.html -c nginx
```

Here, the `-c` flag specifies the container name as `nginx`.