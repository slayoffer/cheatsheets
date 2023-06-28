Yes, Filebeat 7.13.x works with Kubernetes. In fact, Filebeat has native support for ingesting logs from Kubernetes. 

One of the built-in modules for Filebeat is the "Kubernetes" module, which allows users to easily collect logs from Kubernetes clusters. The Kubernetes module is designed to work with the Kubernetes API and is capable of gathering logs from Kubernetes Pods, containers, and system-level logs.

To use the Kubernetes module, you simply need to enable it in the Filebeat configuration file, and the logs will be automatically collected from your Kubernetes environment.

Here are the steps to configure Filebeat to collect logs from Kubernetes:

1. Install Filebeat: Download and install Filebeat from the Elastic website (https://www.elastic.co/downloads/beats/filebeat).

2. Configure the Kubernetes module: In the Filebeat configuration file, locate the `modules` section and uncomment the following lines to enable the Kubernetes module:

```
- module: kubernetes
  enabled: true
  metricsets: ["state_node", "state_deployment", "state_replicaset", "state_pod", "system"]
  hosts: ["https://${KUBERNETES_SERVICE_HOST}:${KUBERNETES_SERVICE_PORT}"]
  bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
  ssl.verification_mode: "none"
  namespace: ${POD_NAMESPACE}
```

This configuration tells Filebeat to use the Kubernetes module, configure the API server endpoint, and specify the bearer token file for authentication.

Note that the `${KUBERNETES_SERVICE_HOST}` and `${KUBERNETES_SERVICE_PORT}` environment variables are automatically set in the Kubernetes environment, and the `${POD_NAMESPACE}` environment variable is also set by the Kubernetes API server.

3. Start Filebeat: Finally, start Filebeat by running the following command:

```
sudo systemctl start filebeat
```

And that's it! Filebeat will now collect logs from your Kubernetes environment using the Kubernetes module.

You can pass the `${KUBERNETES_SERVICE_HOST}` and `${KUBERNETES_SERVICE_PORT}` environment variables to the Filebeat container using Docker Compose. Here's how to modify the `docker-compose.yml` file to include these environment variables:

```
version: '3'
services:
  filebeat:
    image: docker.elastic.co/beats/filebeat:7.13.4
    user: root
    command: filebeat -e -strict.perms=false
    volumes:
      - type: bind
        source: /var/lib/docker/containers
        target: /var/lib/docker/containers
        read_only: true
      - type: bind
        source: /var/log/pods
        target: /var/log/pods
        read_only: true
      - type: bind
        source: /var/run/docker.sock
        target: /var/run/docker.sock
        read_only: true
    environment:
      - OUTPUT_LOGSTASH_HOSTS=your-logstash-host:your-logstash-port
      - KUBERNETES_SERVICE_HOST=${KUBERNETES_SERVICE_HOST}
      - KUBERNETES_SERVICE_PORT=${KUBERNETES_SERVICE_PORT}
```

In this configuration, we're adding the `KUBERNETES_SERVICE_HOST` and `KUBERNETES_SERVICE_PORT` environment variables to the `filebeat` service section. These environment variables will be passed to the Filebeat container when it starts, and Filebeat will use them to connect to the Kubernetes API server.

When you start Docker Compose, you can pass the values for these environment variables using the `-e` flag, like this:

```
export KUBERNETES_SERVICE_HOST=kubernetes.default.svc.cluster.local
export KUBERNETES_SERVICE_PORT=6443
docker-compose up -d
```

This sets the `KUBERNETES_SERVICE_HOST` and `KUBERNETES_SERVICE_PORT` environment variables in your shell session, and then starts Docker Compose.

Note that you will need to replace `your-logstash-host` and `your-logstash-port` with the appropriate values for your Logstash deployment.