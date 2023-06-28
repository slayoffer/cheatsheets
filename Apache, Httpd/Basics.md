### Install

```bash
# for CentOS
sudo yum install httpd -y

# check status
sudo systemctl status httpd
```

### Run

```bash
sudo systemctl enable httpd --now

# restart
sudo systemctl restart httpd
```

### Allow through CentOS firewall

```bash
# for CentOS
firewall-cmd --permanent add-service=http
# or
sudo firewall-cmd --permanent --zone=public --add-port=80/tcp

sudo firewall-cmd --reload
```

### Repo clone

```bash
sudo git clone https://github.com/<application>.git /var/www/html/
```

### Logs

```bash
# for CentOS
cat /var/log/httpd/access_log
cat /var/log/httpd/error_log
```

### Config file

```bash
# CentOS
sudo vim /etc/httpd/conf/httpd.conf

# make separate configs
sudo vim /etc/httpd/conf/httpd.conf

# add lines
Include conf/houses.conf
Include conf/oranges.conf

vim /etc/httpd/conf/houses.conf
vim /etc/httpd/conf/oranges.conf

# change default page (index.html to index.php)
sudo sed -i 's/index.html/index.php/g' /etc/httpd/conf/httpd.conf
```
