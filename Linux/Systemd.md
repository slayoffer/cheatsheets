## Versions of Systemd installed on the system

```bash
systemd --version
```

From the above example, we can clearly see that we have installed Systemd 215 version.

## Check the binary files and libraries of Systemd and Systemctl.

```bash
whereis systemd 
# systemd: /usr/lib/systemd /etc/systemd /usr/share/systemd /usr/share/man/man1/systemd.1.gz
whereis systemctl
# systemctl: /usr/bin/systemctl /usr/share/man/man1/systemctl.1.gz
```

## Check whether systemd is running.

```bash
ps -eaf | grep systemd
```

Note: systemd runs as the parent daemon (PID = 1). In the above command ps, use (-e) to select all processes, (-a) to select all processes except session leaders, and (-f) to select the full format list (that is, -eaf).

Also note: the square brackets in the above example, as well as other examples. The Square Bracket expression is part of grep’s character class pattern matching.

## Analyze the Systemd startup process

```bash
systemd-analyze
# Startup finished in 487ms (kernel) + 2.776s (initrd) + 20.229s (userspace) = 23.493s
```

## Analyze how much time each process spends booting

```bash
systemd-analyze blame
```

### Analyze the key chain at the startup

```bash
systemd-analyze critical-chain

# The time after the unit is active or started is printed after the "@" character.
# The time the unit takes to start is printed after the "+" character.
```

Important: Systemctl accepts service (.service), mount point (.mount), socket (.socket), and device (.device) as units.

## List all available units 

```bash
systemctl list-unit-files
```

## List all running units

```bash
systemctl list-units
```

## List all failed units

```bash
systemctl --failed
```

## Check whether the unit (cron.service) is enabled.

```bash
systemctl is-enabled crond.service
```

## Check whether the unit or service is running.

```bash
systemctl status firewalld.service
```

## List all services (both enabled and disabled)

```bash
systemctl list-unit-files --type=service
```

## How do I start, restart, stop, reload, and check the status of a service (httpd.service) in Linux

```bash
systemctl start httpd.service
systemctl restart httpd.service
systemctl stop httpd.service
systemctl reload httpd.service
systemctl status httpd.service
```

Note: When we use the start, restart, stop and reload commands such as systemctl, we will not get any output on the terminal, only the status command will print output.

## How to activate the service at boot time and enable or disable the service (automatically start the service at boot time)

```bash
systemctl is-active httpd.service
systemctl enable httpd.service
systemctl disable httpd.service
```

## How to mask (so that it cannot start) or unmask services (httpd.service)

```bash
ln -s '/dev/null' '/etc/systemd/system/httpd.service'
systemctl unmask httpd.service
```

## How do I use the systemctl command to stop the service

```bash
systemctl kill httpd
systemctl status httpd
```

Use Systemctl to control and manage mount points

## List all system installation points

```bash
systemctl list-unit-files --type=mount
```

## How to load, unload, reload, reload system load points, and check the status of load points on the system

```bash
systemctl start tmp.mount
systemctl stop tmp.mount
systemctl restart tmp.mount
systemctl reload tmp.mount
systemctl status tmp.mount
```

## How to activate, enable, or disable Mount points at boot time (load automatically at boot time)

```bash
systemctl is-active tmp.mount
systemctl enable tmp.mount
systemctl disable  tmp.mount
```

## How do I mask (make it impossible to boot) or unmask mount points in Linux

```bash
systemctl mask tmp.mount
systemctl unmask tmp.mount
```

Use Systemctl to control and manage sockets

## List all available system sockets.

```bash
systemctl list-unit-files --type=socket
```

## How to start in linux, restart, stop, reload and check the state of the slit (e.g. CUPS.SOCKET)

```bash
systemctl start cups.socket
systemctl restart cups.socket
systemctl stop cups.socket
systemctl reload cups.socket
systemctl status cups.socket
```

### How to activate a socket and enable or disable it at boot time (automatically starting a socket at boot time)

```bash
systemctl is-active cups.socket
systemctl enable cups.socket
systemctl disable cups.socket
```

### How to mask (prevent it from starting) or unmask sockets (cups.socket)

```bash
systemctl mask cups.socket
systemctl unmask cups.socket
```

CPU usage of the service

### Get the current CPU share of the service (such as httpd)

```bash
systemctl show -p CPUShares httpd.service
```

Note: The default value for each service is CPUShare = 1024. You can increase/decrease the CPU share of the process.

### Limit the CPU share of the service (httpd.service) to 2000 CPUShares

```bash
systemctl set-property httpd.service CPUShares=2000
systemctl show -p CPUShares httpd.service
```

Note: When setting CPUSHARE for the service, a directory called service (httpd.service.d) will be created, which contains a file 90-cpushares.conf containing a CPUSHARE LIMIT information. You can treat the file as:

```bash
vi /etc/systemd/system/httpd.service.d/90-CPUShares.conf 
[Service]
CPUShares=2000
```

## Check all the configuration information of the service service

```bash
systemctl show httpd
```

## Analyze the key chain of the service (HTTPD)

```bash
systemd-analyze critical-chain httpd.service
```

### List of dependencies (HTTPD) for services (HTTPD)

```bash
systemctl list-dependencies httpd.service
```

### List the control group according to the level

```bash
systemd-cgls
```

### According to the CPU, memory, input and output list the control group

```bash
systemd-cgtop
```

### How to start the system rescue mode

```
systemctl rescue
```

### How to enter the emergency mode.

```
systemctl emergency
```

### List the current operation level

```
systemctl get-default
```