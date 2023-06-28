### Basics

```bash
# to check load avg
uptime

# or less detailed view
cat /proc/loadavg

# or very detailed view
htop
```

### Explanation

##### Load average: 0.17, 0.32, 0.30

- First number (0.17) for past 1 minute
- Second number (0.32) for past 5 mins
- Third number (0.30) for past 15 mins

*If you have 11 CPUs, then as soon as number shows 11, that means the CPU is 100% busy*

### List info about CPU

```bash
cat /proc/cpuinfo
```

