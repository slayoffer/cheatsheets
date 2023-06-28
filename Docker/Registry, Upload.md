Use Docker hub [official registry image](https://hub.docker.com/_/registry)

And read [docs](https://docs.docker.com/registry/)

Use [this image](https://hub.docker.com/r/konradkleine/docker-registry-frontend) for frontend

### Login into local Nexus registry

```bash
docker login -u admin -p wassup devcraft.app:8082
```

To upload a Docker image to a different Docker registry, you'll need to follow these steps:

1. **Pull the image from the original registry.**
   You can do this with the `docker pull` command:

    ```bash
    docker pull rubius.azurecr.io/dkc-hercules/frontend-prod
    ```

2. **Tag the image with the new registry's URL.**
   This can be done with the `docker tag` command:

    ```bash
    docker tag rubius.azurecr.io/dkc-hercules/frontend-prod dkcimages.azurecr.io/dkc-hercules/frontend-prod
    ```

   In this command, `dkcimages.azurecr.io/dkc-hercules/frontend-prod` is the new tag for your image, which includes the new registry's URL.

3. **Push the image to the new registry.**
   Use the `docker push` command to upload your tagged image:

    ```bash
    docker push dkcimages.azurecr.io/dkc-hercules/frontend-prod
    ```

Please note that you'll need to have appropriate permissions to pull from the original registry and push to the new one. If the registries are private, you may need to log in first with `docker login <registry-url>`. Also, make sure that the names and tags used in these examples match your actual Docker image names and tags.