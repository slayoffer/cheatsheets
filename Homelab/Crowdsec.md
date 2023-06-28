### Install

```bash
curl -s https://packagecloud.io/install/repositories/crowdsec/crowdsec/script.deb.sh | sudo bash
# then
sudo apt install crowdsec
# then
sudo apt install crowdsec-firewall-bouncer-iptables

# install web UI (optional, needs Docker installed and port 3000 open in UFW)
sudo cscli dashboard setup --listen 0.0.0.0
# remove web UI
sudo cscli dashboard remove
```

### Crowdsec app console

```bash
# app.crowdsec.net
sudo cscli console enroll clb68cpnm0000l308ksv8w4cz
```

### Log files

```bash
sudo tail -f /var/log/crowdsec.log
```

### Config file

```bash
sudo vim /etc/crowdsec/config.yaml

# disable WAL notice (optional)
db_config:
  type: sqlite
  use_wal: false
```

### Show bouncers

```bash
sudo cscli bouncers list
```

### Delete bouncer

```bash
sudo cscli bouncers delete FirewallBouncer-1669949826
```



### Metabase bug fix

```bash
sudo adduser workshop crowdsec
sudo chmod g+r /var/lib/crowdsec/data/crowdsec.db
sudo cscli dashboard remove
```

### Enable collections

```bash
/usr/share/crowdsec/wizard.sh -c
```

### Ban list

```bash
sudo cscli decisions list
```

### Unban ip

```bash
sudo cscli decisions delete --id 710 # 710 for id number from the desicions list
```

### Set Crowdsec on Wordpress

```bash
# install crowdsec wp plugin
# set port in plugin settings
# get API key and input into wp plugin settings
sudo cscli bouncers add wordpress-bouncer
```

