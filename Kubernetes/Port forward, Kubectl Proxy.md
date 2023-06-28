## Kubectl Port-Forward Syntax

The syntax of the "kubectl port-forward" command is as follows:

```
kubectl port-forward POD_NAME LOCAL_PORT:REMOTE_POD_PORT
```

kubectl port-forward POD_NAME LOCAL_PORT:REMOTE_POD_PORT

Let's break down the different components of this command:

- **kubectl**: This is the command-line tool used to interact with Kubernetes clusters.
- **port-forward**: This is the action that we want to perform with "kubectl".
- **POD_NAME**: This is the name of the Pod that we want to forward traffic to and from.
- **LOCAL_PORT**: This is the port number on the local machine that we want to use to establish the connection.
- **REMOTE_POD_PORT**: This is the port number on the Pod that we want to connect to.

## Prerequisites

To follow along with the example in this post, we recommend using KodeKloud’s [Kubernetes playground](https://kodekloud.com/blog/r/4ba56734?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab). This playground will provide you instant access to a running Kubernetes cluster with "kubectl" already installed. No need for you to install any software. With just one click, you'll be ready to run the example code snippets and start experimenting right away.

Alternatively, if you prefer to set up your own Kubernetes cluster, you can use a tool such as [minikube](https://kodekloud.com/blog/r/dcbd70e1?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab). Just make sure you have a running Kubernetes cluster with [kubectl](https://kodekloud.com/blog/r/e6715532?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab) connected to it.

## Set up Port Forwarding With Kubectl

In the example below, we will walk through the process of setting up port forwarding in just two simple steps.

### Step 1: Create a Deployment

Open a terminal window and run the following command to create an "nginx" deployment in your cluster. Nginx is a popular open source web server.

```
kubectl create deployment mynginx --image=nginx
```

Here, "mynginx" is the name of the deployment. You can choose any name you like for your deployment. And "--image=nginx" is the name of the Docker image (in this case, "nginx") used to create the container that will run in the Pod you'll be connecting to.

After executing the command, you should see an output similar to this:

Next, verify that the deployment has been created successfully using the "kubectl get deployments" command.

We can see that the deployment named "mynginx" has been deployed successfully and is now running. Now, let's make sure that the Pod created by the deployment is running too. To check this, run the following command:

```
kubectl get pods
```

This command will give you info about all the Pods that are currently running in your cluster, including their names, status, and other useful details. Look for the Pod with a name starting with "mynginx" and ensure that it's in the "Running" state.

The output below shows that the "mynginx-ff886775c-zdrfc" Pod is running successfully. Note that the name of your Pod will be different from ours. That's because Kubernetes creates a Pod name by adding unique characters to the deployment name.

### Step 2: Forward a Local Port to a Port on the Pod

Now that we have our "nginx" web server up and running inside a Pod, we need to figure out a way to access it from our local machine.

Replace the <INSERT_POD_NAME> part in the command below with your Pod’s name, then run it to create a route between your local computer and the Pod:

```
kubectl port-forward <INSERT_POD_NAME> 8080:80
```

After running the command, you’ll see an output similar to the following:

While the terminal session is running, open a web browser and navigate to "[http://localhost:8080](http://localhost:8080/)" to access the "nginx" server running inside the Pod. You should see the default "Welcome to nginx!" page served by the "nginx" web server, indicating that the local request is being forwarded to the Pod.

Congratulations! The port forwarding is working as expected.

Remember that **the terminal session where the "kubectl port-forward" command is running must remain open for the port forwarding to continue working**. If you close the terminal session, the connection between your local machine and the Pod running the "nginx" web server will be lost, and you won't be able to access the "nginx" web server anymore. If you want to continue working on the Kubernetes cluster while maintaining the open connection created by the "kubectl port-forward" command, you should open another terminal window and execute your commands on it.