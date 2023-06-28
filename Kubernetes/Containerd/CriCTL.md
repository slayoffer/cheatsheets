### CMDs

```bash
# crictl is used for debug mostly but also can be used instead of docker when containerd runtime is used for example
crictl pull busybox
crictl images
crictl ps -a
crictl exec -it 3e025dd5@a72d956c4f14881fbb5b1080c9275674e95fb67F965F6478a957d60 1s


```

### List pods

```bash
crictl pods
```

### Runtime

```bash
crictl --runtime-endpoint

export CONTAINER_RUNTIME_ENDPOINT

```