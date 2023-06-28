### Enable firewall

```bash
sudo ufw enable
```

### Show rules

```bash
sudo ufw status
# for numbered
sudo ufw status numbered command
```

### Check apps to config firewall to

```bash
sudo ufw app list
```

### Allow

```bash
sudo ufw allow Apache2
```

### Logs

```bash
sudo cat /var/log/ufw.log
```

### Rate limiting rule check

```bash
# ddos analog
for i in `seq 1 6` ; do curl -w "\n" http://172.28.128:5000 ; done
```

### Add incoming traffic rule

```bash
sudo ufw allow from 95.154.71.237 to any port 22
```

### Add outgoing traffic rule

```bash
sudo ufw allow out to 51.250.11.71
```

### Delete rules

```bash
# To delete a specific rule in ufw, you can use the following command:
sudo ufw delete [rule number] # Replace [rule number] with the number of the rule you want to delete. 

# You can find the rule number by running:
sudo ufw status numbered command

# For example, if you want to delete the rule with number 3, you would run the following command:
sudo ufw delete 3

# If you want to delete all the rules, you can use the reset command:
sudo ufw reset
# This will delete all the rules and reset ufw to its default settings. After running this command, you will need to manually configure your firewall rules again.
```



### Configs location for migration, etc

```bash
/etc/ufw/user.rules
# for ipv6 you can find the rules in 
/etc/ufw/user6.rules
```

### Drop pings

```bash
# Edit the UFW config file
sudo nano /etc/ufw/before.rules

# Add this line of config:
-A ufw-before-input -p icmp --icmp-type echo-request -j DROP
```

### Logs

```bash
sudo ufw status verbose
```

If you get an output saying `Logging: on (low)`, you are good to go but if it shows `Logging: off` as shown above, use the following command to turn on UFW logging:

```bash
sudo ufw logging on
```

Once you have UFW logging on, you can use [the less command](https://linuxhandbook.com/less-command/) to check the UFW firewall logs in your system:

```bash
sudo less /var/log/ufw.log
```

There are 5 levels of UFW logging.

- `off`: Means logging is disabled.
- `low`: Will store logs related to blocked packets that do not match the current firewall rules and will show log entries related to logged rules.
  Yes, you can specify logged rules too, and will show you how in the later part of this guide.
- `medium`: In addition to all the logs offered by the low level, you get logs for invalid packets, new connections, and logging done through rate limiting.
- `high`: Will include logs for packets with rate limiting and without rate limiting.
- `full`: This level is similar to the high level but does not include the rate limiting.

Now, if you want to change your default or the current level of logging, you just have to follow the given command structure:

```bash
sudo ufw logging logging_level
```

So if I want to change my current logging level to `medium`, it can be doe using the given command:

```bash
sudo ufw logging medium
```

To add the logging rule, you just have to follow the command syntax:

```bash
sudo ufw allow log service_name
```

For example, I have added a log rule for port no. 22 (SSH):

```bash
sudo ufw allow log 22/tcp
```
