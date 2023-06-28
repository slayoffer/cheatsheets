### Set admin account

```bash
docker compose exec passbolt su -m -c "/usr/share/php/passbolt/bin/cake \
passbolt register_user \
-u slayoffer@gmail.com \
-f <yourname> \
-l <surname> \
-r admin" -s /bin/sh www-data
```

