### 301 redirect for nexus.rubius.com

```bash
server {
    listen 443 ssl;
    server_name www.nexus.rubius.com;
    ssl_certificate /certs/rubius.com/fullchain.pem; серты не понадобились для редиректа
    ssl_certificatei_key /certs/rubius.com/privkey.pem;
    return 301 https://nexus.rubius.com$request_uri;
}

# doesnt work
server {
    listen 443 ssl;
    server_name "~^www\.(.*)$";
    return 301 $scheme://$1$request_uri;
}

# old and bad practice
server {
    server_name example.com *.example.com;
        if ($host ~* ^www\.(.+)) {
            set $raw_domain $1;
            rewrite ^/(.*)$ $raw_domain/$1 permanent;
        }
        # [...]
    }
}
```

### Full Nginx config

```bash
server {
    listen 80;
    server_name *.nexus.rubius.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name www.nexus.rubius.com;
    return 301 $scheme://nexus.rubius.com$request_uri;
}


server {
    listen 443 ssl;
    server_name .nexus.rubius.com;

    ssl_certificate /certs/rubius.com/fullchain.pem;
    ssl_certificate_key /certs/rubius.com/privkey.pem;

    ssl_stapling on;
    ssl_stapling_verify on;

    add_header Strict-Transport-Security "max-age=31536000";

    access_log /var/log/nginx/nexus.rubius.com.log;
    error_log /var/log/nginx/error_nexus.rubius.com.log;

    # allow large uploads of files - refer to nginx documentation
    client_max_body_size 1G;
    proxy_send_timeout 120;
    proxy_read_timeout 300;
    proxy_buffering    off;
    proxy_request_buffering off;
    keepalive_timeout  5 5;
    tcp_nodelay        on;

    # optimize downloading files larger than 1G - refer to nginx doc before adjusting
    #proxy_max_temp_file_size 2G;

    location / {
        proxy_pass http://localhost:8081/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto "https";
    }
```

### rubius.com

```bash
server {
#    if ($host = rubius.com) {
#        return 301 https://$host$request_uri;
#    } # managed by Certbot

    listen 80;
    server_name rubius.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl;
    server_name www.rubius.com;
    ssl_certificate /etc/letsencrypt/live/rubius.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/rubius.com/privkey.pem; # managed by Certbot
    return 301 $scheme://rubius.com$request_uri;
}

server {
    listen 443 ssl http2;

    server_name rubius.com;
    ssl_certificate /etc/letsencrypt/live/rubius.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/rubius.com/privkey.pem; # managed by Certbot

    ssl_stapling on;
    ssl_stapling_verify on;

    access_log /var/log/nginx/rubius.com.log;
    error_log /var/log/nginx/error_rubius.com.log;

    location /customdevelopment {
	return 301 /software;
    }
 
    location /customdevelopment/ru {
	return 301 /software;
    }

    location /portfolio {
	return 301 /projects;
    }
 
#    location /products {
#	return 301 /;
#    }
 
    location /point-cloud {
        return 301 /projects/construction-monitoring-system-based-on-point-cloud-analysis;
    }
 
    location /about {
	return 301 /company/about;
    }
 
    location /vacancies {
        return 301 /company/career;
    }
 
    location /about/ru {
         return 301 /company/about;
    }
 
    location /contacts/ru {
	return 301 /contacts;
    }

    location /point-cloud/ru {
        return 301 /projects/sistema-kontrolya-stroitelstva-na-osnove-analiza-oblaka-tochek;
    }
 
    location /3d-cadastre {
        return 301 /projects/system-for-automatic-generation-of-3d-cadastral-certificates;
    }
 
    location /mcad-ecad-integration {
        return 301 /projects/ecad-and-mcad-integration-for-end-to-end-pcb-design;
    }
 
    location /mcad-ecad-integration/ru {
	return 301 /projects/integraciya-ecad-i-mcad-dlya-skvoznogo-proektirovaniya-pechatnyh-plat;
    }

#   location /ru {
#	rewrite ^ / permanent;
#    }
#
#    location /products/ru {
#	return 301 /;
#    }

    location /files {
	alias /usr/local/apps/www.rubius.com/files/;
	try_files $uri $uri/ =404;
    }

    location /.well-known/pki-validation {
        alias /usr/local/apps/www.rubius.com/.well-known/pki-validation/;
        try_files $uri $uri/ =404;
    }

    location / {
#        set $frame_options '';
#        if ($http_referer !~ '^https?:\/\/([^\/]+\.)?(rubius\.com|webvisor\.com|metri[ck]a\.yandex\.(com|ru|com\.tr))\/'){
#            set $frame_options 'SAMEORIGIN';
#        }
#        add_header X-Frame-Options $frame_options;

        proxy_pass                  http://127.0.0.1:5610/;

        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
        proxy_set_header   Upgrade          $http_upgrade;
        proxy_set_header   Connection       $http_connection;
    }
}
```
