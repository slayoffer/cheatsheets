### Create VM

```bash
yc compute instance create \
--name demo-1 \
--metadata-from-file user-data=startup.sh \
--create-boot-disk image-folder-id=standard-images,image-family=ubuntu-2004-lts \
--zone ru-central1-a \
--network-interface subnet-name=default-ru-central1-a,nat-ip-version=ipv4
```

### List VMs

```bash
yc compute instance list
```

