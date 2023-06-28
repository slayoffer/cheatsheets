### Update package index

```bash
sudo apt update 
```

### Inspect package details

```bash
apt info nala

```

### Install package

```bash
sudo apt install nala
```

### Display help information

```
$ nala --help

```

### Display version

```
$ nala --version
```

### Search for specific package

```
$ nala search --names ^mc$

```

### Make a more general search and display full package description

```
$ nala search --full "collect powerups"

```

### Show package info

```
$ nala show heroes

```

### Install a package

```
$ sudo nala install --assume-yes mc

```

### Update package index and install a package

```
$ sudo nala install --update --assume-yes git

```

### Remove a package

```
$ sudo nala remove --assume-yes git

```

### Purge a package

```
$ sudo nala purge --assume-yes mc

```

### Install specific package version

```
$ sudo nala install --update --assume-yes bash=5.0-6ubuntu1

```

### Show all package versions

```
$ nala show --all-version bash
```

### Display history

```
$ sudo nala history

```

### Display specific history entry

```
$ sudo nala history info 1

```

### Redo an operation

```
$ sudo nala history --assume-yes redo 2

```

### Undo an operation. Notice, you can only redo/undo specific operations

```
$ sudo nala history --assume-yes undo 3
Error: 'history undo' for operations other than install or remove are not currently supported
```

### Display history entry.

```
$ sudo nala history info 2

```

### Undo an operation

```
$ sudo nala history --assume-yes undo 2

```

### Clean package cache

```
$ nala clean
Cache has been cleaned
```