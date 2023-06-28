### Create dummy file named Dummy worth of 100MB

```bash
dd if=/dev/zero of=Dummy bs=1MB count=100
```

### Format disk and fil it with zero mock files

```bash
umount /dev/sdb1

sudo dd if=/dev/zero of=/dev/sdb bs=1M
# but its recommended to use /dev/random instead of /dev/zero if want to format the disk and make it a lot harder to recover old data
```

