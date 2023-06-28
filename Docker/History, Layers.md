### Check image layers (Dockerfile content)

```bash
docker history image_name --not-trunc

# or run CT and check
docker inspect --format '{{json .RootFS.Layers}}' demo-app:v1
```

### Get inside CT files without running it

```bash
cat /var/lib/docker/aufs/ct_id/ct_inside_file
# or
cat /var/lib/docker/overlay2/ct_id/ct_inside_file
```

### Get layers of image

```bash
# If the image contains a shell, you can run an interactive shell container using that image and explore whatever content that image has. If sh is not available, the busybox ash shell might be.

# For instance:
docker run -it image_name sh

# Or following for images with an entrypoint
docker run -it --entrypoint sh image_name

# Or if you want to see how the image was built, meaning the steps in its Dockerfile, you can export layers:
docker image history --no-trunc image_name > image_history.txt
```

### Debug

```bash
docker image build --rm=false --no-cache .

docker container diff ...
```

### Best practices

-   Delete temporary file in the same step where they are created
-   Small changes to big files are big changes
-   Merge your `RUN` commands together