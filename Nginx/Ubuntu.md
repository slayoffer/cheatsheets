### Install

```bash
sudo apt install nginx
```

### Config files

```bash
cd /etc/nginx
/etc/nginx/sites-enabled # for turned on websites
/etc/nginx/sites-available # for available websites
```

### Enable website

```bash
# create symlink
sudo ln -s /etc/nginx/sites-available/acmesales.com /etc/nginx/sites-enabled/acmesales.com

sudo systemctl reload nginx
```

### Deploy website

```bash
# for single website replace contents of
/var/www/html

# for separate website add
sudo mkdir /var/www/acmesales.com

# config
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/acmesales.com
sudo vim etc/nginx/sites-available/acmesales.com

# remove default_server from listen section
listen 80;
listen [::]:80;
# add
server_name acmesales.com www.acmesales.com;
# change website dir to
root /var/www/acmesales.com
```

