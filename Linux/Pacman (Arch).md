# Pacman is for Arch Linux

### Refresh package database

```bash
sudo pacman -Syy # -S for sync, -y to refresh local database cash, second -y to force refresh
```

### Repos location

```bash
cat /etc/pacman.d/mirrorlist
```

### Install package

```bash
sudo pacman -S htop
```

### Remove package

```bash
sudo pacman -R htop
```

### Search package in Arch repo

```bash
pacman -Ss pygame # -s for search
```

### Show orphan packages

```bash
pacman -Qdt # -Q for query package database, -d to skip dependency check, -t to limit results for orphans only
```

### Upgrade packages

```bash
sudo pacman -Syu # -u for upgrade
```

### How to reset/update mirrorlist

```bash
cd /etc/pacman.d
# backup mirrorlist
sudo cp mirrorlist mirrorlist.bak

# Empty current mirrorlist file (use with care)
sudo truncate -s 0 /etc/pacman.d/mirrorlist

# update file with the new mirrorlist link from Arch website

# Update repository index (after updating mirrorlist file)
sudo pacman -Syyy
```

### Show installed packages

```bash
expac --timefmt='%F %T' '%l %n'
# or
expac --timefmt='%F %T' '%l %n' | sort -n | tail -n 5
```

