

## List open ports in Linux

```bash
sudo lsof -i -P -n | grep LISTEN
sudo netstat -tulpn | grep LISTEN
sudo ss -tulpn | grep LISTEN
sudo lsof -i:22 ## see a specific port such as 22 ##
sudo nmap -sTU -O IP-address-Here
```

## Find the process that is using a specific port

```
lsof -P | grep ':8080'
```

### Check app port usage

```bash
sudo netstat -natulp | grep postgres | grep LISTEN
sudo lsof -i -P -n
sudo lsof -i -P -n | grep LISTEN
lsof -i -P -n | grep LISTEN # OpenBSD #
sudo ss -tulpn | grep :3306
sudo lsof -n -i :80 | grep LISTEN
```

### Kill app which uses port

```bash
sudo fuser -k 10174/tcp
```



### Viewing the Internet network services list

```bash
less /etc/services
```

### Testing if a port is open from a bash script

```bash
# One can use the “/dev/tcp/{HostName}_OR_{IPAddrress}>/{port}” syntax to check if a TCP port is open on a Linux or Unix machine when using Bash. In other words, the following is Bash specific feature. Let us see if TCP port 22 is open on localhost and 192.168.2.20:
(echo >/dev/tcp/localhost/23) &>/dev/null && echo "open" || echo "close"
(echo >/dev/tcp/192.168.2.20/22) &>/dev/null && echo "open" || echo "close"

# script
#!/bin/bash
dest_box="aws-prod-server-42"
echo "Testing the ssh connectivity ... "
if ! (echo >/dev/tcp/$dest_box/22) &>/dev/null
then
    echo "$0 cannot connect to the $dest_box. Check your vpn connectivity."
else
    echo "Running the ansible playboook ..."
    ansible-playbook -i hosts --ask-vault-pass --extra-vars '@cluster.data.yml' main.yaml
fi
```



### Open ports 1-1024 for non-root service users

```bash
# 1 way
sudo setcap CAP_NET_BIND_SERVICE=+eip /path/to/binary

# 2 way

# create config file in service dir
vim /etc/systemd/system/your.service./capabilities.conf

[Service]
AmbientCapabilities=CAP_NET_BIND_SERVICE

systemctl daemon-reload

# 3 way

# localhost
sudo iptables -t nat -I OUTPUT -p tcp -d 127.0.0.1 --dport 80 -j REDIRECT --to-ports 8080
# external
sudo iptables -t nat -I PREROUTING -p tcp --dport 80 -j REDIRECT --to-ports 8080
```



## Close open ports in Linux

To close the port, first, you will need to stop the service and to find the service name, you can use the same ss command with `-p` option:

```
sudo ss -tulnp | grep LISTEN
```

As you can see, the NGINX is utilizing port number 80. So let's stop it using the given command:

```
sudo systemctl stop nginx
```

As it will enable itself on every boot and you can alter this behavior using the given command:

```
sudo systemctl disable nginx
```

For better results, I would recommend changing firewall rules.

```
sudo ufw status
```

And if it shows `inactive`, you can use the given command to enable it:

```
sudo ufw enable
```

Now, you just have to pair the `deny` option with the `port number`:

```
sudo ufw deny 80
```
