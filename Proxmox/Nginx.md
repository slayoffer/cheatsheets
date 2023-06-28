### Nginx config

```bash
vim /etc/nginx/sites-enabled/proxmox.conf

nginx -t

systemctl restart nginx

upstream proxmox {
    server pve.devcraft.app;
}
 
#server {
 #   listen 80 default_server;
  #  rewrite ^(.*) https://$host$1 permanent;
#}
 
server {
    listen 443 ssl;
    server_name pve.devcraft.app;
    #ssl on;
    ssl_certificate /etc/letsencrypt/live/pve.devcraft.app/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/pve.devcraft.app/privkey.pem; # managed by Certbot
    proxy_redirect off;
    location / {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade"; 
        proxy_pass https://localhost:8006;
        proxy_buffering off;
        client_max_body_size 0;
        proxy_connect_timeout  3600s;
        proxy_read_timeout  3600s;
        proxy_send_timeout  3600s;
        send_timeout  3600s;
    }

}

server {
    if ($host = pve.devcraft.app) {
        return 301 https://$host$request_uri;
    } # managed by Certbot



        listen 80;

        server_name pve.devcraft.app;

        location / {

        proxy_pass https://10.0.0.1:8006$request_uri;
        }


}

server {
    if ($host = nextcloud.devcraft.app) {
        return 301 https://$host$request_uri;
    } # managed by Certbot



 #       listen 80;
#
 #       server_name nextcloud.devcraft.app;
#
 #       location / {
#
 #       proxy_pass http://localhost:8080;
#        }


}

server {
    listen 443 ssl;
    server_name nextcloud.devcraft.app;
    #ssl on;
    location / {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_pass http://95.154.71.237:8080;
        proxy_buffering off;
        client_max_body_size 0;
        proxy_connect_timeout  3600s;
        proxy_read_timeout  3600s;
        proxy_send_timeout  3600s;
        send_timeout  3600s;
    }

    ssl_certificate /etc/letsencrypt/live/nextcloud.devcraft.app/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/nextcloud.devcraft.app/privkey.pem; # managed by Certbot
}
```

### SSL

```bash
# SSL starter
vim /etc/nginx/sites-enabled/proxmox.conf

server {
    listen 443 ssl;
    server_name pve.devcraft.app;
    #ssl on;
    proxy_redirect off;
    location / {
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade"; 
        proxy_pass https://localhost:8006;
        proxy_buffering off;
        client_max_body_size 0;
        proxy_connect_timeout  3600s;
        proxy_read_timeout  3600s;
        proxy_send_timeout  3600s;
        send_timeout  3600s;
    }
}

# then
sudo apt update

sudo apt install python3-certbot-nginx

sudo certbot --nginx -d nextcloud.devcraft.app -d nextcloud.devcraft.app

# Only valid for 90 days, test the renewal process with
certbot renew --dry-run
```

