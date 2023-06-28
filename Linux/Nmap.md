### Scan IPs

```bash
nmap 192.168.1.38
# or
nmap nextcloud.devcraft.app

# verbose
nmap -v 192.168.1.38

# scan several ips
nmap 192.168.1.38 192.168.1.87

# scan range of ips
nmap 192.168.1.1-90
```

### Exclude IPs

```bash
nmap 192.168.1.1-90 --exclude 192.168.1.87
```

### Show with service vesrions

```bash
nmap -sV 192.168.1.38
```

### Show with detailed info about host

```bash
nmap -A 192.168.1.38
```

### Scan whole subnet

```bash
nmap 192.168.0.0/24

# check the time for this cmd
time nmap 192.168.1.0/24

# quick scan
time nmap -T5 192.168.1.0/24 # -T5 for fastest check mode, default is T3, starts from T0
```

### Short information scan

```bash
nmap -sP 192.168.1.38
```

