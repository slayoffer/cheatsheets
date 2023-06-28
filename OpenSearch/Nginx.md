Absolutely! Here's an example of enabling the Nginx module in Filebeat using Docker Compose:

1. Create a `docker-compose.yml` file with the following contents (make sure to update the Elasticsearch host and port to match your environment):

```
version: '3'

services:
  filebeat:
    image: docker.elastic.co/beats/filebeat:7.13.4
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
      - MODULES=system,nginx
    volumes:
      - ./filebeat.yml:/usr/share/filebeat/filebeat.yml
      - /var/log:/var/log
      - /var/lib/docker/containers:/var/lib/docker/containers:ro
```

In the above example, we are defining a service named "filebeat" and passing two environment variables. The `ELASTICSEARCH_HOSTS` variable sets the Elasticsearch output host and port, and the `MODULES` variable is used to enable the `system` and `nginx` modules. We are also mounting three volumes to the container - the Filebeat configuration file, the host's log directory, and the Docker containers directory.

2. Create a `filebeat.yml` file at the same level as the `docker-compose.yml` file with the following contents:

```
filebeat.config:
  modules:
    path: ${path.config}/modules.d/*.yml
    module:
      enabled: true
      modules.d: ${path.config}/modules.d/*.yml
output.elasticsearch:
  hosts: '${ELASTICSEARCH_HOSTS}'
```

In the above example, we are setting the module path to the default path using `${path.config}` and setting the `enabled` parameter to `true`. Additionally, we set the Elasticsearch output hosts using the `${ELASTICSEARCH_HOSTS}` environment variable.

3. Create a `modules.d` directory at the same level as the `filebeat.yml` file and add the following `nginx.yml` file with the contents:

```
module:
  enabled: ${MODULES_NGINX:false}
  access:
    enabled: true
    var.paths: ['/var/log/nginx/access.log*']
  error:
    enabled: true
    var.paths: ['/var/log/nginx/error.log*']
```

In the above module configuration, we set `enabled` to `${MODULES_NGINX}` environment variable, which we passed in the `docker-compose.yml` file to enable or disable the Nginx module. The Nginx module is configured to collect access and error logs from the default Nginx log directory.

4. Start the Filebeat container by running the following command:

```
docker-compose up -d
```

This starts the Filebeat container in daemon mode with the configuration options defined in the `docker-compose.yml` file, enabling the Nginx and System modules.