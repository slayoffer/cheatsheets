### Check devnull

```bash
stat /dev/null

# It is empty
# The creation date is the same as last boot
# The file's UID and GID are 0
# And every user is allowed to read and write on /dev/null
# It is a character device file which means it will act like an unbuffed device and can accept data streams
```

### Redirect stdout and show only errors

```basg
command 1> /dev/null
```

### Redirect errors and show only stdout

```bash
command 2> /dev/null
```

### Redirect all

```bash
command 2>&1 /dev/null
```

