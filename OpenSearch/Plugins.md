### Working with plugins

To use the OpenSearch image with a custom plugin, you must first create a [`Dockerfile`](https://docs.docker.com/engine/reference/builder/). Review the official Docker documentation for information about creating a Dockerfile.

```bash
FROM opensearchproject/opensearch:latest
RUN /usr/share/opensearch/bin/opensearch-plugin install --batch <pluginId>
```

Then run the following commands:

```bash
# Build an image from a Dockerfile
docker build --tag=opensearch-custom-plugin .
# Start the container from the custom image
docker run -p 9200:9200 -p 9600:9600 -v /usr/share/opensearch/data opensearch-custom-plugin
```

Alternatively, you might want to remove a plugin from an image before deploying it. This example Dockerfile removes the security plugin:

```bash
FROM opensearchproject/opensearch:latest
RUN /usr/share/opensearch/bin/opensearch-plugin remove opensearch-security
```

You can also use a Dockerfile to pass your own certificates for use with the [Security Plugin](https://opensearch.org/docs/latest/security/):

```bash
FROM opensearchproject/opensearch:latest
COPY --chown=opensearch:opensearch opensearch.yml /usr/share/opensearch/config/
COPY --chown=opensearch:opensearch my-key-file.pem /usr/share/opensearch/config/
COPY --chown=opensearch:opensearch my-certificate-chain.pem /usr/share/opensearch/config/
COPY --chown=opensearch:opensearch my-root-cas.pem /usr/share/opensearch/config/
```