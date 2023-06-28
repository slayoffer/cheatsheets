## Create a Pod

Let's begin by launching your [code editor](https://kodekloud.com/blog/r/349ead1d?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab). Next, create a new directory. Within this directory, make a new file with a `.yaml` extension. Copy and paste the following content into the new `.yaml` file:

```
apiVersion: apps/v1
kind: Deployment
metadata:
 name: mysql-deployment
spec:
 replicas: 1
 selector:
   matchLabels:
     app: mysql
 template:
   metadata:
     labels:
       app: mysql
   spec:
     containers:
     - name: mysql-container
       image: mysql:8.0
```

In this file, we have defined [a Deployment](https://kodekloud.com/blog/r/fe25335b?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab) named `mysql-deployment` that is responsible for managing a single Pod. Withing this Pod, there will be one container running the `mysql:8.0` image. The Deployment will ensure that the Pod is always kept running. If the Pod crashes for some reason, the Deployment will automatically recreate the Pod, maintaining the desired count (which in this case is set to 1).

Now run the command below to create the Deployment:

```
kubectl apply -f <YOUR-FILE-NAME>.yaml
```

Replace `<YOUR-FILE-NAME>` with the name of your file.

Executing the command above will produce the following output:

![](https://ci3.googleusercontent.com/proxy/1OYP2w1PxF331ck82Zpt2TNsNVukQr6rR8rLZ-WEboJ81heKbItDECKajAFQsUpNtVV86115FqYNqAu-ffTDAuh5X6K5DFLFV_V4PoRZtM0hYuaxpCkgSXnwBq8nPy10pxqnHa6rCvPVry9N5eEO1YKOISXBe-taWYVEbg=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/05/data-src-image-7b564a68-a2c4-48a9-9852-b21c225295b0.png)

As you can see, a Deployment named `mysql-deployment` has been created. Next, let’s check the Pod status by running the following command:

```
kubectl get pods
```

This will generate an output similar to the following:

![](https://ci4.googleusercontent.com/proxy/T7qBnxXmtwdeURNL0uhs3hvPQBvkBxfbz6dUNhJp_hje_N6c8EC0HTgXo_DxSVDFfZQFrp7S54vXh9D-_eteLRkwW3hJRSoOBhTb435_IXYFjvzcgQJ3BiLaqBfOFlCe_HblmYwa015n7vAtzxZLxiTLeHiG3fuyQXcWgw=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/05/data-src-image-8a3a5ad2-a591-4089-99d7-9522291b300a.png)

As you can see, the Pod has a `CrashLoopBackOff` status as highlighted above.

**Note**: Running the `kubectl get pods` command immediately after creating the Deployment may not display the `CrashLoopBackOff` status. Instead, you might see a different status like `Error`, for example. This variation occurs because the Pod goes through different states during its lifecycle as it starts up, runs, and potentially encounters issues. So, wait for up to five minutes or so before running the `kubectl get pods` command again, and you should see the `CrashLoopBackOff` status.

Now, let's focus on the following three columns of the output above and examine their meaning:

- **`READY`**: This indicates the number of containers in the Pod that are ready. `0/1` means there is 1 container in the Pod, and 0 of them are ready. This suggests that the container is not yet running as expected.
- **`STATUS`**: This shows the current status of the Pod. **`CrashLoopBackOff` means the container in this Pod is failing and is being restarted, over and over again.**
- **`RESTARTS`**: This indicates how many times the container within the Pod has been restarted. The number 5 suggests that the container has crashed and been restarted five times.

The `CrashLoopbackOff` status is a clear indication that something has gone wrong with the Pod. Let’s troubleshoot to find out the cause of this problem.

## Debugging CrashLoopBackOff

Kubernetes offers various mechanisms for troubleshooting issues. In this section, we'll explore two steps that will guide you through the debugging process and help you resolve the `CrashLoopBackOff` problem.

### Step 1: Using `kubectl describe pod` to get Pod details

To address the issue with our Pod, the first step is to get detailed Pod information. We might find some helpful clues that can guide us in the right direction.

Run the following command to fetch Pod details:

```
kubectl describe pod <POD-NAME>
```

Replace the `<POD-NAME>` with the name of your Pod.

Running the command above will produce an output similar to the following:

![](https://ci3.googleusercontent.com/proxy/92-BdTLJZuinb-0FIGYcBxB7oCboOvMBnfRY2lsswoXS-AoaY5CVLsHVuF7Ert-tdLVqccDttAUFJm5RIU5YjwFpxDr46YPncJG2iP9q_qjTL6vgy7X6XiDpV5KsL_8mEum5AzhJhDlG9Kfe2wO_Mrm8KF9NY7h6j81hSA=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/05/data-src-image-eee17d41-38a1-482d-a125-020d9da5e435.png)

Note that the screenshot above shows only a partial section of the complete output.

Within the `Containers` section of the output, we see that the container is in a `Waiting` state due to `CrashLoopBackOff`, with an exit code of 1. Typically, an exit code of 0 means that the process ran successfully without errors, while **a non-zero exit code indicates some sort of error**. However, in this case, the exit code does not provide us with sufficient information to tell us what we should do next.

To gather more insights, scroll down to the `Events` section.

![](https://ci5.googleusercontent.com/proxy/Zf42hwg57eKMLRtgynpDEB-Hj8DL3PQehc_swJUWBZdUBY0B1JTwjZjtEHingpqxsYxXjTIAFzTTqwFmbXwnYScMuzOmiTx1HnZ64AqyB0YWS27paOwh-x0KWrHzcEtlwF68gFAgLr32oCRfF6eT8pz356kfkfpj6x39mA=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/05/data-src-image-eaa9dfac-b964-4f84-bb2e-b59240209310.png)

In the last line of the `Events` section, we find the statement `Back-off restarting failed container`. This means that the kubelet has attempted to restart the container after a failure but has been unsuccessful. As a result, the kubelet will make another attempt to restart the container. Now, you might wonder, **what does "Back-off" mean**?

Kubernetes, by default, restarts a Pod when it crashes. If the application doesn’t recover after the restart, Kubernetes doesn’t immediately attempt to restart the Pod again. Instead, it waits for some time.

Why? Because it hopes the waiting time resolves any temporary issue the application may have. If the issue still persists, Kubernetes increases the waiting time between each restart attempt, up to a certain limit. This approach is referred to as the back-off strategy.

Unfortunately, the `Events` section does not have any helpful information regarding why the container fails to start. Therefore, our next step is to inspect the Pod logs for any potential clues regarding the issue.

### Step 2: Using `kubectl logs` to get Pod logs

Pod logs are records of events within an application. They can often provide valuable insights into any issues the application is facing. To fetch these logs, run the following command:

```
kubectl logs <POD-NAME>
```

Running the command above will produce the following output:

![](https://ci6.googleusercontent.com/proxy/yCCQqGDZ-C_6Ea9VM0LDwLU0KqvP20V5bVs8gwtSYkUPfG81I_LIZqvKkGBy7_iVq8o8UYPqnNt7nnqy8-6mD6i8UcJasVNHeUPO51BTR81DizsGIR2qL5DNRxMxnC_nY-WOG8QGAIG4mYRF9a4nZFO8HamOy7honiDzfA=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/05/data-src-image-baccd173-214e-4c75-a0e1-493215015b0f.png)

**The logs clearly indicate the reason for the error — we have not set the password in our Deployment configuration**. To resolve this issue, one of the options is to define the `MYSQL_ROOT_PASSWORD` in the environmental variables section of our Deployment configuration. Let’s do that now.

Add the following details to the `containers` block of the `template.spec` in your configuration file. Note that you can provide any value you prefer for the password.

```
env:
- name: MYSQL_ROOT_PASSWORD
  value: secret-password
```

The final configuration file should look like this:

```
apiVersion: apps/v1
kind: Deployment
metadata:
 name: mysql-deployment
spec:
 replicas: 1
 selector:
   matchLabels:
     app: mysql
 template:
   metadata:
     labels:
       app: mysql
   spec:
     containers:
     - name: mysql-container
       image: mysql:8.0
       env:
       - name: MYSQL_ROOT_PASSWORD
         value: secret-password
```

**Note**: For demonstration purposes, we stored the password in the configuration file. In real-world scenarios, it is not recommended to store sensitive information, like passwords, in Deployment configuration files for security reasons. Instead, you should use a specific Kubernetes object called [Secrets](https://kodekloud.com/blog/r/b1023279?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab).

After updating the Deployment configuration file, run the following command to apply the changes:

```
kubectl apply -f <your-file-name>.yaml 
```

After running the command above, you’ll see the following output:

![](https://ci3.googleusercontent.com/proxy/Fn7G_5mcd27uMgZC9XmZJ9_8cCfFxd_OlPEyiNr5wmhLjqWVj_8HLWrC_EX3vSXCCDb4Zrbjf9VRgZKcoGpO0LELwpvJHDSrRe2to31JO_8nC6DfAbmhkBDbIW29RjLvmgh3ipThrOFSZFTGqliFIC96v_ikUOwsHikSjw=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/05/data-src-image-0fd1546c-7caf-456d-b09a-6c17a886724c.png)

As you can see Kubernetes has updated the existing Deployment with the new configuration. Next, let’s run the following command to check the Pod status:

```
kubectl get pods
```

![](https://ci5.googleusercontent.com/proxy/nZzxLk1UBRuU_PI8StUoUUuiizM3ejfgtnvub-AbOQH1IayNSh_nSsUoCVodYWYXRF8x3EWJdXURBkvgIQjE4NjQ_jMSOOPapKi4N1P3k1v9X_edDibBAAZ7ouZhN189oLR86pJ_tEPYzfsqMkfdBTB-i4e_Uq95s4TV5g=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/05/data-src-image-0a9f7b03-628a-4dea-8dcd-ce7e8a4d2396.png)

As you can see the Pod is up and running as expected. We have resolved the `CrashLoopbackOff` state of the Pod.