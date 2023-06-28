### CPU

```bash
# check load average
uptime # 1 min, 5 min, 15 min load
# if 1-5 min count is more than number of cpus then need to investigate

# check processes which take too much cpu
top
# or better
htop
```

### RAM

```bash
# check ram
free -hm

# check dynamic ram usage
vmstat 1 5 # 1 for delay (sec), 5 for query count. Will poll data five time with 1 sec delay
# check r and b columns, shouldnt be too high
# also check free memory and swapping

# check processes
ps -efly --sort=-rss | head # -efly for long format, -rss for resident set size column (non-swappable physical memory)

# on prod server best solution is to rollback the memory-hog process to prev version
```

### Disk load (high iowait)

```bash
iostat -xz 1 20 # poll stats every second for 20 times. -xz for active devices only and extended stats format
# then look for device name with high iowait, w/s and utilization stats

# find process which makes high iowait
sudo iotop -oPab # flag to show only active processes with accumulative stats in batch mode

# check PID, process name and IO columns with high IO
```

### Disk space

```bash
# check free space
df -h

# find large files
sudo find / -type f size +100M -exec du -ah {} + | sort -hr | head
# or use ncdu

# find process which is making the file take too much space
sudo lsof /var/log/some.log

# check logs of process or CT which owns this process
# set logrotate
```

### Connection refused or timeout

```bash
# check website
curl http://api.smith.lab:8080

# find process which is listening to port
sudo ss -l -n -p | grep 8080 # -l for all listen sockets, -n not to resolve service names like HTTP and SSH, -p to show process

# dont use netstat instead of ss - its obsolete

# check traffic on host
sudo tcpdump -ni any tcp port 8080 # -n not to resolve host or port names, -i for network interface
# if shows S flag then server can be reached, but if server responds with R (reset) flag then smth is wrong with it

# then check Docker logs, Inspect CT, check exposed internal ports, Docker proxy for traffic forward issues
```

### DNS failure

```bash
# check DNS config
sudo cat /etc/resolv.conf

# check where unknown DNS requests are sent (upstream DNS resolver)
resolvectl dns # will show upstream DNS resolver IP

# check local DNS resolve server
dig db.slayo.serv # it will only check local DNS resolver

# check upstream DNS resolver in case local doesnt know the address
dig @10.0.2.3 db.slayo.serv # @10.0.2.3 for upstream DNS resolver
# to only show ip
dig +short @10.0.2.3 db.slayo.serv

# check if local and upstream DNS resolvers are working OK with other websites
dig google.com

# if ok then:
    # check A records on DNS hosting for db.slayo.serv
    # check webserver config for syntax errors
    # restart DNS server
```

### Trace process (in case nothing else helps)

```bash
sudo strace -s 128 -p 19419 # -s for output size of 128 bytes, -p for PID

# get summary of process system calls
sudo strace -p 28485 -c # -c for summary stats
# check errors column

# focus errors system call
sudo strace -p 28485 -e openat # openat is system call which caused errors
# will show file connected to error and error code
# if error code is -1 it means that file doesnt not exist

# for dynamic library calls made
ltrace

# for kernel-level issues
dtrace
```



### Check logs

```bash
cd /var/log

# main system and process log
cat /var/log/syslog

# user log
cat /var/log/auth.log

# hardware logs
cat /var/log/kern.log
cat /var/log/dmesg.log

# combined way
sudo journalctl -r # -r for reverse order (from new to old)

# timed view
sudo journalctl -r --since "2 hours ago"

# service view
sudo journalctl -r -u ssh

# priority view
sudo journalctl -r -u ssh -p err # show errors first

# regex view
sudo journalctl -r -u ssh -g "session opened" # if -g doesnt work use pipe with grep
```

### Parse logs

```bash
grep "10.0.2.33" /var/log/syslog

# show 5 logs before the incident
grep -B 5 "user NOT in sudoers" /var/log/auth.log # -B for before

# show server access IPs
sudo awk '{pring $1}' /var/log/nginx/access.log # will show only IPs column from which website was accessed

# check exact server errors
sudo awk '($9 ~ /500/)' /var/log/nginx/access.log # look for 500 errors in 9 column

# check exact server errors with condition
sudo awk '($9 ~ /404/) {if (/POST/) print}' /var/log/nginx/access.log # shows 404 errors only if finds POST string in column

# check either 404 or 401 error
sudo awk '($9 ~ /404|401/)' /var/log/nginx/access.log
```

