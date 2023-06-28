### Install on Ubuntu

```bash
sudo apt install apache2
```

### Ports config

```bash
sudo vim /etc/apache2/ports.conf
```

### Website config

```bash
sudo vim /etc/apache2/sites-available/nextcloud.devcraft.app.conf
```

### Enable or disable website

```bash
# enable
a2ensite

# disable
a2dissite
```

### Plugins

```bash
# enable module (implicit)
a2enmod

# disable
a2dismod

# list plugins
apt search libapache2-mod

# for php plugin
sudo apt install libapache2-mod-php8.1

# for python plugin
sudo apt install libapache2-mod-python

# list built-in modules
apache2 -l

# for explicit mod enabling
sudo a2enmod php8.1
sudo systemctl restart apache2
```

