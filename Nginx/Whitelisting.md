### Whitelist

```bash
server {
    listen 127.0.0.1:80;
    server_name 127.0.0.1;
    location /nginx_status {
        stub_status on;
        access_log off;
        allow 127.0.0.1; # allow
        allow ::1; # for zabbix agent v2 (ipV6)
        deny all; # drop
    }
}  
```

