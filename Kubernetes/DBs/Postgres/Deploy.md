## **Key Takeaways**

- By deploying Postgres on Kubernetes, you can benefit from the best of both worlds: a powerful and versatile database and a flexible and robust platform.
- You can use Kubernetes features like stateful sets, persistent volumes, and headless services to ensure the high availability and durability of your Postgres database.
- You can use environment variables, secrets, and other tools to configure, secure, and back up your Postgres database on Kubernetes.

By the end of this article, you will have a fully functional Postgres database running on Kubernetes that can handle the load and demands of your web application. Let’s get started!

## **Setting up a Kubernetes cluster**

The first step is to set up a Kubernetes cluster where you will deploy your Postgres database. You can use any cloud provider or platform that supports Kubernetes, such as Google Cloud Platform (GCP), Amazon Web Services (AWS), Microsoft Azure, or DigitalOcean. Alternatively, you can use a [local Kubernetes cluster set up using minikube](https://kodekloud.com/blog/r/eb6f9c7e?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab). For this article, we will use GCP as an example.

Want to gain fundamental and foundation knowledge on Google Cloud Services (GCP)? Check out this [GCP Cloud Digital Leader Certification](https://kodekloud.com/blog/r/506529da?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab) course.

To create a Kubernetes cluster on GCP, you need to have a Google account and [install the Google Cloud SDK](https://kodekloud.com/blog/r/bb48fe02?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab) on your machine. Then, you can use the following commands to create a cluster named `postgres-cluster` with three nodes:

```
gcloud config set project <your-project-id>
gcloud config set compute/zone <your-zone>
gcloud container clusters create postgres-cluster num--nodes=3
```

This may take a few minutes to complete. You can verify that the cluster is running using the following running:

```
gcloud container clusters describe postgres-cluster

kubectl get nodes
```

You should see something like this:

![](https://ci6.googleusercontent.com/proxy/2M2mOKt5Vq02g5C1LPNQ4alc5E4S088Fn53CZUXVHqUuuMpaqmTEPJoYHNnhD1SXSrfOCS7KPIUL3fuXsgksP45b0P2YoolMr92qNdgSRMOo2TozWPVMiqETyAxixKeTHbcAi69HjjfKuAuzTduv97hhOv6nWRF_YG3qbQ=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/06/data-src-image-8d8ea8c8-3f14-4cfe-b791-de9b06f5b751.png)

Now you have a Kubernetes cluster ready to deploy your Postgres database.

## **Creating a Persistent Volume and a Persistent Volume Claim for Postgres Data**

The next step is to create a persistent volume and a persistent volume claim for Postgres data. A persistent volume (PV) is a piece of storage that can be used by a Pod to store data and exists beyond the lifecycle of a Pod. It allows data to persist even if the Pod is destroyed. This enables the data to be preserved and reused by other Pods.

A persistent volume claim (PVC) is a request for a PV that matches certain criteria, such as size, access mode, and storage class.

To create a PV and a PVC for Postgres data, you need to create a YAML file that defines them. For example, you can create a file named `postgres-pv.yaml` with the following content:

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: postgres-pv
spec:
  capacity:
    storage: 10Gi 
  accessModes:
    - ReadWriteOnce 
  persistentVolumeReclaimPolicy: Retain 
  gcePersistentDisk: 
    pdName: postgres-disk
    fsType: ext4 
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: postgres-pvc
spec:
  accessModes:
    - ReadWriteOnce 
  resources:
    requests:
      storage: 10Gi 
  storageClassName: "" 
  volumeName: postgres-pv 
```

This YAML file defines a PV named `postgres-pv` with a size of 10 GB, a [read-write-once access mode](https://kodekloud.com/blog/r/50568ba7?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab), a [retain reclaim policy](https://kodekloud.com/blog/r/518503d7?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab), and a GCP disk as the underlying storage. It also defines a PVC named `postgres-pvc` that requests a PV with the same specifications.

Before applying this YAML file, you need to create a GCP disk named `postgres-disk` with the same size and file system type as the PV. You can do this by running:

`gcloud compute disks create postgres-disk --size=10GB --type=pd-standard --zone=<your-zone>`

Then, you can apply the YAML file by running:

`kubectl apply -f postgres-pv.yaml`

This will create the PV and the PVC in your cluster. You can verify them by running:

```
kubectl get pv
kubectl get pvc
```

Now you have a PV and a PVC ready to store your Postgres data.

## **Deploying Postgres as a Stateful Set with a Headless Service**

The next step is to deploy Postgres as a stateful set with a headless service. A stateful set is a Kubernetes resource that manages the deployment and scaling of a [set of Pods](https://kodekloud.com/blog/r/8cc8933a?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab) that have persistent identities and storage. A headless service is a Kubernetes service that does not have a cluster IP address. It instead provides DNS records for the Pods in the stateful set.

To deploy Postgres as a stateful set with a headless service, you need to create another YAML file that defines them. For example, you can create a file named `postgres-ss.yaml` with the following content:

```
apiVersion: v1
kind: Service
metadata:
  name: postgres 
  labels:
    app: postgres
spec:
  selector:
    app: postgres
  ports:
    - port: 5432 
  clusterIP: None
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: postgres
spec:
  serviceName: postgres
  replicas: 1
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      containers:
        - name: postgres
          image: postgres:13
          env:
            - name: POSTGRES_USER 
              value: postgres
            - name: POSTGRES_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: postgres-secret
                  key: password
            - name: PGDATA
              value: /var/lib/postgresql/data/pgdata
          ports:
            - containerPort: 5432
              name: postgres
          volumeMounts:
            - name: postgres-pv-claim
              mountPath: /var/lib/postgresql/data
  volumeClaimTemplates:
    - metadata:
        name: postgres-pv-claim
      spec:
        accessModes:
          - ReadWriteOnce
        resources:
          requests:
            storage: 10Gi
```

This YAML file defines a headless service named `postgres` that exposes port 5432 and selects Pods with the label `app: postgres`. It also defines a stateful set named "postgres" that uses the headless service, has one replica, and creates Pods with the label `app: postgres`. Each Pod has a container named postgres that runs the image `postgres:13`, sets some environment variables for Postgres configuration, exposes port 5432, and mounts a volume from the PVC template named `postgres-pv-claim`.

Before applying this YAML file, you need to create a secret named `postgres-secret` that contains the Postgres password. You can do this by running:

`kubectl create secret generic postgres-secret --from-literal=password=<your-password>`

Then, you can apply the YAML file by running:

`kubectl apply -f postgres-ss.yaml`

**Note**: _To learn more about Kubernetes Secrets, check out our blog post: [Kubernetes Secrets](https://kodekloud.com/blog/r/ea5b66eb?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab)._

The command above will create the headless service and the stateful set in your cluster. You can verify them by running:

```
kubectl get svc
kubectl get sts
kubectl get pods
kubectl get pvc -l app=postgres
```

Now you have a Postgres database running on Kubernetes as a stateful set with a headless service.

## **Configuring Postgres Parameters and Credentials**

The next step is to configure Postgres parameters and credentials. You can use environment variables, config maps, and secrets to customize the Postgres configuration and provide the Postgres credentials.

In the previous step, we already used some environment variables to set the Postgres user, password, and data directory. You can also use other environment variables to set other Postgres parameters, such as POSTGRES_DB to specify the default database name, POSTGRES_INITDB_ARGS to pass arguments to initdb, or POSTGRES_INITDB_WALDIR to specify a separate WAL directory. You can find the full list of supported environment variables in the [Postgres Docker image documentation](https://kodekloud.com/blog/r/585018ae?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab).

You can also use config maps to store Postgres configuration files, such as `postgresql.conf` or `pg_hba.conf`, and mount them as volumes in the Postgres container. For example, you can create a config map named `postgres-config` with the following command:

`kubectl create configmap postgres-config --from-file=postgresql.conf --from-file=pg_hba.conf`

This assumes that you have the `postgresql.conf` and `pg_hba.conf` files in your current directory. Then, you can modify the stateful set YAML file to add a volume mount for the config map:

```

      containers:
        - name: postgres
          ...
          volumeMounts:
            - name: postgres-pv-claim
              mountPath: /var/lib/postgresql/data
            - name: postgres-config
              mountPath: /etc/postgresql
  ...
  volumeClaimTemplates:
    - metadata:
        name: postgres-pv-claim
      spec:
        ...
  volumes:
    - name: postgres-config 
      configMap:
        name: postgres-config
```

This will mount the config map as a volume in the /etc/postgresql directory of the Postgres container.

You can also use secrets to store Postgres credentials, such as passwords or certificates, and pass them as environment variables or mount them as volumes in the Postgres container. For example, we already used a secret named `postgres-secret` to store the Postgres password and pass it as an environment variable. You can also use secrets to store SSL certificates and keys for encrypting Postgres connections. For example, you can create a secret named `postgres-ssl` with the following command:

`kubectl create secret generic postgres-ssl --from-file=server.crt --from-file=server.key`

This assumes that you have the `server.crt` and `server.key` files in your current directory. Then, you can modify the stateful set YAML file to add a volume mount for the secret:

```
containers:
  - name: postgres
    ...
    env:
      - name: POSTGRES_USER
        value: postgres
      - name: POSTGRES_PASSWORD
        valueFrom:
          secretKeyRef:
            name: postgres-secret
            key: password
      - name: PGDATA
        value: /var/lib/postgresql/data/pgdata
      - name: POSTGRES_SSLMODE
        value: verify-ca 
      - name: POSTGRES_SSLCERT
        value: /etc/ssl/server.crt
      - name: POSTGRES_SSLKEY
        value: /etc/ssl/server.key
    ...
    volumeMounts:
      - name: postgres-pv-claim
        mountPath: /var/lib/postgresql/data
      - name: postgres-config 
        mountPath: /etc/postgresql 
      - name: postgres-ssl
        mountPath: /etc/ssl 
    ...
  volumes:
    - name: postgres-config 
      configMap:
        name: postgres-config 
    - name: postgres-ssl 
      secret:
        secretName: postgres-ssl 
```

This will mount the secret as a volume in the /etc/ssl directory of the Postgres container and set some environment variables for Postgres SSL configuration.

By using environment variables, config maps, and secrets, you can configure Postgres parameters and credentials according to your needs. You can find more details and examples in the [Postgres Docker image documentation](https://kodekloud.com/blog/r/94c63abb?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab)

## **Connecting to Postgres from your web application**

The next step is to connect to Postgres from your web application. You can use any programming language or framework that supports Postgres, such as Python, Ruby, Java, Node.js, Django, Rails, Spring Boot, Express, etc. You can also use any Postgres client or driver that supports SSL, such as psycopg2, pg, JDBC, etc.

To connect to Postgres from your web application, you need to know the following information:

- **The hostname of the Postgres Pod**. Use the DNS name of the stateful set, which is `<statefulset-name>`. `<headless-service-name>`. In our case, it is `postgres.postgres`.
- **The port of the Postgres Pod**. Use the port of the headless service, which is 5432.
- **The database name of the Postgres Pod**. Use the environment variable POSTGRES_DB or the default value `postgres`.
- **The user name of the Postgres Pod**. Use the environment variable POSTGRES_USER or the default value `postgres`.
- **The password of the Postgres Pod**. Use the secret `postgres-secret` or the value you set for it.
- **The SSL mode of the Postgres Pod**. Use the environment variable POSTGRES_SSLMODE or the default value prefer.
- **The SSL certificate and key files of the Postgres Pod**. Use the secret `postgres-ssl` or the files you set for it.

If you are using Python and psycopg2 as your web application and Postgres client, you can connect to Postgres with the code below.

_If you don’t have Python installed, you can execute the code below on our [Python 3.8 Playground](https://kodekloud.com/blog/r/087b1d05?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab). Quickly jump in and test the connection right on your browser!_

```
import psycopg2

params = {
    "host": "postgres.postgres",
    "port": 5432,
    "dbname": "postgres",
    "user": "postgres",
    "password": "<your-password>",
    "sslmode": "verify-ca",
    "sslcert": "/etc/ssl/server.crt",
    "sslkey": "/etc/ssl/server.key"
}

conn = psycopg2.connect(**params)

cur = conn.cursor()

cur.execute("SELECT version()")
version = cur.fetchone()
print(f"Postgres version: {version}")

cur.execute("SELECT * FROM books")
books = cur.fetchall()
print(f"Books: {books}")

cur.close()
conn.close()
```

This code will connect to Postgres and print out information from the database.

You can also use environment variables or secrets to pass the connection parameters to your web application. For example, you can create a config map named `webapp-config` with the following command:

```
kubectl create configmap webapp-config --from-literal=host=postgres.postgres --from-literal=port=5432 --from-literal=dbname=postgres --from-literal=user=postgres --from-literal=sslmode=verify-ca --from-literal=sslcert=/etc/ssl/server.crt --from-literal=sslkey=/etc/ssl/server.key
```

Then, you can modify your web application deployment YAML file to add environment variables from the config map:

```
containers:
  - name: webapp
    ...
    envFrom:
      - configMapRef:
          name: webapp-config
    env:
      - name: password
        valueFrom:
          secretKeyRef:
            name: postgres-secret
            key: password 
    ...
    volumeMounts: 
      - name: postgres-ssl 
        mountPath: /etc/ssl 
    ...
  volumes:
    - name: postgres-ssl 
      secret: 
        secretName: postgres-ssl 
```

This will pass the connection parameters to your web application as environment variables and mount the SSL certificate and key files as a volume.

By using these methods, you can connect to Postgres from your web application and perform any operations you need. You can find more details and examples in the [Postgres documentation](https://kodekloud.com/blog/r/94b1c24b?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab).

## **Scaling up and down Postgres Pods**

The next step is to scale up and down Postgres Pods. You can use kubectl to scale up and down the stateful set that manages the Postgres Pods. For example, you can run the following command to scale up the stateful set to three replicas:

`kubectl scale sts postgres --replicas=3`

This will create two more Postgres Pods in your cluster. You can verify their creation by running the command below:

`kubectl get pods -l app=postgres`

You should see something like this:

![](https://ci3.googleusercontent.com/proxy/dfHXf5bUBPmYXzEAo0q8ThnNXg0duOk5t3hfuaws2uHEgxNgyDlVDFtcoWv3JAzF2HcpFpG58vwuakzObcPESdzW2O6eWRdrc9BQd8zxbjWGqzURUlUN1fraJ5RCg1g2z1ZQifrFgqrhiVYqz038StsbB56jVu6Y329jWw=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/06/data-src-image-89af12c6-623e-4f25-b68e-23f48bce0f8b.png)

You can also run the following command to scale down the stateful set to one replica:

`kubectl scale sts postgres --replicas=1`

This will delete two Postgres Pods from your cluster. You can verify it by running:

`kubectl get pods -l app=postgres`

You should see something like this:

![](https://ci3.googleusercontent.com/proxy/EQMXLuqgZUAE4OaFzyRCnS-hmOxKcU8_rLyqADKp-7gwIPbZkzDvFDw9qCHQEbck8rEZxc0GC_yYJjHGSUNgq2lrQf_zgFOmiJ1zDarVxsGE7prFqzBUp7DyfbPsFy4qoPvhEMPFWln8Car3Mq5DP_3EcBn2xdmKiGG9Gg=s0-d-e1-ft#https://kodekloud.com/blog/content/images/2023/06/data-src-image-e8e4842f-4307-4728-bf9b-28a2be9210e8.png)

By using these commands, you can scale up and down Postgres Pods according to your needs. However, you should be aware that scaling up and down Postgres Pods does not automatically create or delete Postgres databases or users. You need to manually create or delete them using Postgres commands or tools. You should also be aware that this does not automatically configure replication or load balancing among them. You need to manually configure them using other Postgres and Kubernetes tools.

## **Performing backups and restores of Postgres data**

The final step is to perform backups and restores of Postgres data. You can use various methods and tools to backup and restore Postgres data, such as pg_dump, pg_restore, pg_basebackup, pgBackRest, etc. You can also use Kubernetes tools such as [kubectl cp](https://kodekloud.com/blog/r/1cccb29f?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab), [kubectl exec](https://kodekloud.com/blog/r/f6fb7164?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab), Velero, etc., to backup and restore Postgres data.

For example, you can use pg_dump and kubectl cp to backup a Postgres database to your local machine. You can run the following command to dump the database named `postgres` from the Pod named `postgres-0` to a file named `backup.sql`:

`kubectl exec -it postgres-0 -- pg_dump -U postgres -d postgres > backup.sql`

This will prompt you for the Postgres password and then dump the database to the file. Then, you can run the following command to copy the file from the Pod to your local machine:

`kubectl cp postgres-0:backup.sql backup.sql`

This will copy the file from the Pod to your current directory.

_**Note:** To get an in-depth understanding of the kubectl cp command, check out our blog post: [kubectl cp: How to Copy File From Pod to Local (With Examples)](https://kodekloud.com/blog/r/d06c5ccc?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab)._

You can also use pg_restore and kubectl cp to restore a Postgres database from your local machine. You can run the following command to copy the file from your local machine to the Pod:

`kubectl cp backup.sql postgres-0:backup.sql`

This will copy the file from your current directory to the Pod. Then, you can run the following command to restore the database named `postgres` from `backup.sql` on the Pod named `postgres-0`:

`kubectl exec -it postgres-0 -- pg_restore -U postgres -d postgres < backup.sql`

This will prompt you for the Postgres password and then restore the database from the file.