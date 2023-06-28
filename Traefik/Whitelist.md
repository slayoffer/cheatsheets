To add the `ipWhiteList` middleware as labels to a Docker container running a web service, follow these steps:

1. Make sure Traefik is running with Docker provider enabled and configured properly.
2. Add the appropriate labels to your web service's Docker container configuration in your `docker-compose.yml` file or when running the container with `docker run`. Here's an example for a web service called `my-web-service`:

In `docker-compose.yml`:

```
yamlCopy codeversion: '3'

services:
  my-web-service:
    image: your-web-service-image
    labels:
      - "traefik.enable=true"
      - "traefik.http.middlewares.default-whitelist.ipwhitelist.sourcerange=192.168.1.1/32, 10.0.0.1/32, 2001:0db8:85a3::/64"
      - "traefik.http.routers.my-web-service.rule=Host(`example.com`)"
      - "traefik.http.routers.my-web-service.middlewares=default-whitelist"
      - "traefik.http.routers.my-web-service.entrypoints=web, websecure"
      - "traefik.http.routers.my-web-service.tls.certresolver=my-cert-resolver"
    networks:
      - traefik-public

networks:
  traefik-public:
    external: true
```

Replace `your-web-service-image` with your web service's Docker image name, and `example.com` with your domain.