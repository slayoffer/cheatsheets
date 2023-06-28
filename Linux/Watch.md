### Check files

```bash
watch ls
```

### Check storage

```bash
watch df -h
```

### Check RAM

```bash
watch free -m
```

### Highlight changes

```bash
watch -d free -m
```

### Edit watch update time

```bash
watch -d -n 0.5 free -m # from 2 to 0.5 secs
```

### Watch with piping

```bash
watch 'ls -lh | grep some_file.txt' # quotes needed
```

