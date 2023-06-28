### To turn off in case of large files

```bash
location /api/ {
    proxy_pass http://backend_server;
    proxy_buffer off; # might be useful for Nexus
}
```

