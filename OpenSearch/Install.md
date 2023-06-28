### Preliminary

```bash
# check java version (for OS 2.+ java should be version 11 or 17)
./jdk/bin/java -version

# To use different Java version, set OPENSEARCH_JAVA_HOME or JAVA_HOME env to Java install location
export OPENSEARCH_JAVA_HOME=/path/to/opensearch-2.5.0/jdk

# Disable memory paging and swapping performance on the host to improve performance
sudo swapoff -a

# check, value should be at least 262144
cat /proc/sys/vm/max_map_count

# if lower set
sudo vim /etc/sysctl.conf
# add line or replace with it
vm.max_map_count=262144
# reload
sudo sysctl -p

# or
sudo sysctl -w vm.max_map_count=262144

# open ports
443
5601
9200
9250
9300
9600
```

### Run with CT

```bash
 # map ports 9200 and 9600, set the discovery type to "single-node" and requests newest image
docker run -d -p 9200:9200 -p 9600:9600 -e "discovery.type=single-node" opensearchproject/opensearch:latest
 
 # or
 docker run \
  -p 9200:9200 -p 9600:9600 \
  -e "discovery.type=single-node" \
  -v /path/to/custom-opensearch.yml:/usr/share/opensearch/config/opensearch.yml \
  opensearchproject/opensearch:latest
 
 # check
 curl http://localhost:9200 -ku 'admin:admin'
```

### Run with Compose

```yaml
version: '3'
services:
  opensearch-node1:
    image: opensearchproject/opensearch:latest
    container_name: opensearch-node1
    environment:
      - cluster.name=opensearch-cluster # Name the cluster
      - node.name=opensearch-node1 # Name the node that will run in this container
      - discovery.seed_hosts=opensearch-node1,opensearch-node2 # Nodes to look for when discovering the cluster
      - cluster.initial_cluster_manager_nodes=opensearch-node1,opensearch-node2 # Nodes eligibile to serve as cluster manager
      - bootstrap.memory_lock=true # Disable JVM heap memory swapping
      - "OPENSEARCH_JAVA_OPTS=-Xms512m -Xmx512m" # Set min and max JVM heap sizes to at least 50% of system RAM
      - "DISABLE_INSTALL_DEMO_CONFIG=true" # Prevents execution of bundled demo script which installs demo certificates and security configurations to OpenSearch
      - "DISABLE_SECURITY_PLUGIN=true" # Disables security plugin
    ulimits:
      memlock:
        soft: -1 # Set memlock to unlimited (no soft or hard limit)
        hard: -1
      nofile:
        soft: 65536 # Maximum number of open files for the opensearch user - set to at least 65536
        hard: 65536
    volumes:
      - opensearch-data1:/usr/share/opensearch/data # Creates volume called opensearch-data1 and mounts it to the container
    ports:
      - 9200:9200 # REST API
      - 9600:9600 # Performance Analyzer
    networks:
      - opensearch-net # All of the containers will join the same Docker bridge network
  opensearch-node2:
    image: opensearchproject/opensearch:latest
    container_name: opensearch-node2
    environment:
      - cluster.name=opensearch-cluster # Name the cluster
      - node.name=opensearch-node2 # Name the node that will run in this container
      - discovery.seed_hosts=opensearch-node1,opensearch-node2 # Nodes to look for when discovering the cluster
      - cluster.initial_cluster_manager_nodes=opensearch-node1,opensearch-node2 # Nodes eligibile to serve as cluster manager
      - bootstrap.memory_lock=true # Disable JVM heap memory swapping
      - "OPENSEARCH_JAVA_OPTS=-Xms512m -Xmx512m" # Set min and max JVM heap sizes to at least 50% of system RAM
      - "DISABLE_INSTALL_DEMO_CONFIG=true" # Prevents execution of bundled demo script which installs demo certificates and security configurations to OpenSearch
      - "DISABLE_SECURITY_PLUGIN=true" # Disables security plugin
    ulimits:
      memlock:
        soft: -1 # Set memlock to unlimited (no soft or hard limit)
        hard: -1
      nofile:
        soft: 65536 # Maximum number of open files for the opensearch user - set to at least 65536
        hard: 65536
    volumes:
      - opensearch-data2:/usr/share/opensearch/data # Creates volume called opensearch-data2 and mounts it to the container
    networks:
      - opensearch-net # All of the containers will join the same Docker bridge network
  opensearch-dashboards:
    image: opensearchproject/opensearch-dashboards:latest
    container_name: opensearch-dashboards
    ports:
      - 5601:5601 # Map host port 5601 to container port 5601
    expose:
      - "5601" # Expose port 5601 for web access to OpenSearch Dashboards
    environment:
      - 'OPENSEARCH_HOSTS=["http://opensearch-node1:9200","http://opensearch-node2:9200"]'
      - "DISABLE_SECURITY_DASHBOARDS_PLUGIN=true" # disables security dashboards plugin in OpenSearch Dashboards
    networks:
      - opensearch-net

volumes:
  opensearch-data1:
  opensearch-data2:

networks:
  opensearch-net:
  
# or add custom config files from local host to CT

```

### To make custom local volume available to Docker

```bash
sudo chown -R 1000:1000 ./custom-opensearch.yml

# example
services:
  opensearch-node1:
    volumes:
      - opensearch-data1:/usr/share/opensearch/data
      - ./custom-opensearch.yml:/usr/share/opensearch/config/opensearch.yml # custom local volume
  opensearch-node2:
    volumes:
      - opensearch-data2:/usr/share/opensearch/data
      - ./custom-opensearch.yml:/usr/share/opensearch/config/opensearch.yml # custom local volume
  opensearch-dashboards:
    volumes:
      - ./custom-opensearch_dashboards.yml:/usr/share/opensearch-dashboards/config/opensearch_dashboards.yml # custom local volume
```

