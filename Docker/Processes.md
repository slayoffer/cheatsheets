### List processes in CT

```bash
docker exec ct_name ps -eaf
```

### Restart process in CT

Of course it's **recommended to rebuild** the container when you do the changes, but I recommend `reload`ing the `nginx` service with either running `bash` inside of container and then reload it:

```bash
docker exec -it NGINX_NAME bash
```

Then run `service nginx reload`.

Or:

```bash
docker exec -it NGINX_NAME service nginx reload
```