### Install

```bash
dpkg -i telnet.deb
```

### Remove

```bash
dpkg -r telnet.deb
```

### List installed packs

```bash
dpkg -l telnet
```

### Check package status

```bash
dpkg -s telnet
```

### Details about package

```bash
dpkg -p telnet
```

### Export list of installed packages

```bash
dpkg --get-selections > packages.list
```

### Show all dpkgs installed

```bash
zcat /var/log/dpkg.log.* | grep -i "installed"
# or
zgrep "installed" /var/log/dpkg.log*

# with app name
zcat /var/log/dpkg.log.* | grep -i "installed" | grep "google-cloud
```
