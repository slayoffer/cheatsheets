### Set 24H time format

```bash
sudo localectl set-locale LC_TIME="en_GB.UTF-8"
```

## Checking the Current Time Zone

```bash
timedatectl

# check time & date
date
```

The system time zone is configured by symlinking the `/etc/localtime` file to a binary time zone’s identifier in the `/usr/share/zoneinfo` directory.

Another way to check the time zone is to view the path the symlink points to using the [`ls`](https://linuxize.com/post/how-to-list-files-in-linux-using-the-ls-command/) command:

```bash
ls -l /etc/localtime
```

## Change time zone

```bash
timedatectl list-timezones
# or
ls /usr/share/zoneinfo

# then
sudo timedatectl set-timezone Asia/Vladivostok

# then check/confirm
timedatectl
```

### Set NCP service for time sync (only for systemd OS)

```bash
sudo timedatectl set-ntp true
```

The message "RTC in local TZ: no" usually appears during the boot process of a computer and indicates that the system is not using the Real-Time Clock in the local time zone. This can happen if the system is configured to use Universal Time Coordinated (UTC) instead of the local time zone.

To change the system to use the local time zone, you can modify the RTC configuration. The steps to do this depend on the operating system you are using. For example, on Linux systems, you can use the `timedatectl` command to set the system clock to use the local time zone. Here is an example command:

`sudo timedatectl set-local-rtc 1`

sudo apt-get update
sudo apt-get install ntp