### Get into Redis CLI

```bash
docker exec -it redic_ct red is-cli
```

### Set key

```bash
set name "Name"
```

### Set temp key

```bash
set nametemp "Name" EX 10 # will disappear in 10 sec
```

### Check key

```bash
exists name
```

### Delete key

```bash
del name
```

### Append key

```bash
append name "Name"
get name
append name "John"
get name
```

### Subscribe and publish

```bash
subscribe newvideos
# log in to another shell, get into Redis CLI and run
publish newvideos "REDIS TEST"
```

### Delete all

```bash
FLUSHALL
```

