### Dockerfile

```bash
# Install Nginx.
RUN \
add-apt-repository -y ppa:nginx/stable && \
apt update && \
apt install -y nginx && \
rm -rf /var/lib/apt/lists/* && \
echo "\ndaemon off;" >> /etc/nginx/nginx.conf && \
chown -R www-data:www-data /var/lib/nginx
# Define mountable directories.
VOLUME ["/etc/nginx/sites-enabled", "/etc/nginx/certs", "/etc/nginx/config",
# Define working directory.
WORKDIR /etc/nginx
# Define default command.
CMD ["nginx"]
```

