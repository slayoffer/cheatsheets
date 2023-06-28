### Install snap

```bash
sudo apt update

sudo apt install snapd
```



### List snap packages

```bash
snap find appname
```

### Install

```bash
sudo snap install nmap
```

### Check app folder

```bash
which nmap # snap apps are stored in the /snap/bin/*
```

### Remove app

```bash
sudo snap remove nmap
```

### Update app

```bash
sudo snap refresh nmap

# to update all snap apps
sudo snap refresh
```

