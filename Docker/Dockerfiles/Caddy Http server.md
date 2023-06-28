### Dockerfile

```bash
FROM debian:buster-20220527
RUN apt update && \ 
    apt install -y curl debian-keyring debian-archive-keyring apt-transport-https && \
    curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/gpg.key' | gpg --dearmor -o /usr/share/keyrings/caddy-stable-archive-keyring.gpg && \
    curl -1sLf 'https://dl.cloudsmith.io/public/caddy/stable/debian.deb.txt' | tee /etc/apt/sources.list.d/caddy-stable.list && \
    apt update && apt install -y caddy
WORKDIR /caddy
COPY ./Caddyfile /caddy/Caddyfile
EXPOSE 8080/tcp
ENTRYPOINT ["caddy","run"] 
```

