This how-to describes how to safely upgrade your Ubuntu server while keeping the dockerized application stack (and docker daemon) on top of that at the same version. This was specifically done for Ubuntu 18.04 LTS to Ubuntu 20.04 LTS but will work on other version upgrades in the same manner.

If you’re a System operator you’ll probably know Ubuntu 18.04 LTS ended it’s security updates by the end of April 2023. A distribution upgrade on it’s own is not that complex. However, when you want the software running on top of that, in this example a dockerized application, to stay the same there might be some pitfalls. I did this for one of our customers and I thought it could be usefull. So it was published for you to easily avoid issues.

 

## Preparations

**Note:** Run these commands as root, or prefix them with `sudo `.

We first need to take some precautions not to break anything during the upgrade. Be warned, you should always have some form of backup.

First we stop the docker service. This is to prevent data corruption (mounts and/or settings) but also in case of a heavy application stacks, this speeds the reboots.![img]()

```
$ service docker stop
$ service docker status
```

 

Check for docker versions currently installed, and note those down.

```
dpkg --list | grep docker
hi  docker-ce                              5:20.10.11~3-0~ubuntu-bionic                    amd64        Docker: the open-source application container engine
hi  docker-ce-cli                          5:20.10.11~3-0~ubuntu-bionic                    amd64        Docker CLI: the open-source application container engine
ii  docker-ce-rootless-extras              5:23.0.4-1~ubuntu.18.04~bionic                  amd64        Rootless support for Docker.
ii  docker-scan-plugin                     0.23.0~ubuntu-bionic                            amd64        Docker scan cli plugin.
ii  golang-docker-credential-helpers       0.5.0-2ubuntu0.1                                amd64        Use native stores to safeguard Docker credentials
ii  python3-docker                         2.5.1-1                                         all          Python 3 wrapper to access docker.io's control socket
ii  python3-dockerpycreds                  0.2.1-1                                         all          Python3 bindings for the docker credentials store API
```

 

Unhold packages that are pinned down. **(link to glossary: pinned)**

```
$ apt-mark showhold
docker-ce
docker-ce-cli
salt-common
salt-minion

$ apt-mark unhold salt-common salt-minion docker-ce docker-ce-cli
Canceled hold on salt-common.
Canceled hold on ...
<snip>

# WARN: There could be others, evaluate them here before continuing
```

 

Move foreign repository files to `.old` directory.

```
mkdir /etc/apt/sources.list.old/
mv /etc/apt/sources.list.d/* /etc/apt/sources.list.old/
```

 

Refresh repository index.![img]()

```
apt-get update
```

 

# Upgrade test

**WARNING**: Preferably run `do-release-upgrade` in a console and not over SSH.

Do release upgrade check then ***ABORT***. This is to see which foreign packages to remove by checking the upgrade log.

```
# Do upgrade start, then ABORT.  
$ do-release-upgrade

# Before giving definite "yes" abort upgrade, then search for "Foreign" in log
$ less /var/log/dist-upgrade/main.log

# found -> Obsolete: containerd.io docker-ce docker-ce-cli docker-ce-rootless-extras docker-scan-plugin
```

 

Then we uninstall the foreign packages.

**WARNING**: Do NOT use the option `--purge`, this could break the docker container (application) stack because it fully removes the directory `/var/lib/docker/`.

```
# docker removal
$ apt-get remove containerd.io docker-ce docker-ce-cli docker-ce-rootless-extras docker-scan-plugin

# salt minion removal
$ apt-get remove salt-common salt-minion

# (optional) telegraf removal
$ apt-get remove telegraf
```

 

*Your mileage may vary here when uninstalling any other foreign packages. What happens in a nutshell with the foreign packages is:*

- *You uninstall them (but preserve the data)*
- *You upgrade the OS*
- *You then install the same version of the package (that is compiled against the new OS version and its underlying dependant libraries).*

Ok, on to the actual upgrade.

![img]()

# ![img]()Upgrade final

Then you continu to the actual release upgrade.

```
$ do-release-upgrade

# Automatically restart services (like pamd)
Yes

# Keep local config files. Overwrite with package maintainers newer version
No

# Follow on screen questions
```

 

*If you provision server systems with a configuration management and orchestration tool, in our case Saltstack, then you should already have the newer features from the package maintainer configuration files templated for that distro. The installation of the new packages below shows the manual installation of said packages for illustrating the purpose of this article. You can of course do this with your preferred provisioning and management configuration tool*

Do a reboot to finalize the OS upgrade.![img]()

```
# Reboot by pressing a 'y' in the console, or do it manually :)
$ reboot 
```

 

Final cleanup of old packages if applicable.

```
apt autoremove
```

 

# Install new foreign packages

You now reinstall the foreign packages with the same or newer version, as proposed by the producer of said packages.

**Docker**

For this example we installed the same docker version but for Ubuntu 20.04. Add de repository, as decribed here: https://docs.docker.com/engine/install/ubuntu/

```
# update apt repository index
$ apt-get update
```

 

After adding the Docker repository, check for the version you previously wrote down (see above).

```
# Check for version
apt-cache madison docker-ce | awk '{ print $3 }'
```

 

Continu installation for ‘specific version’ as decribed here: https://docs.docker.com/engine/install/ubuntu/

 

**Saltstack minion**

At CAP5 we use a custom minion install script for this that also checks and sets hostname, firewall connections, DNS and other items needed.![img]()

```
$ /root/install_minion.sh <SERVER_NAME>.example.com <SALT_SERVER>.example.com 3005.1
```

A simple check on the salt master should show whether it is up again:

```
$ salt <SERVER_NAME>.example.com test.ping
```

 

**Other packages**

Of course there probably are packages that you run on your servers. They should be removed as described in a proper … Then add them again as advised by producer / vendor of the software.

**WARNING:** By all means check if there are release notes or upgrade procedures for your software to avoid loss of data.

# Issues

We’ve noticed some ‘funny stuff’ that was resolved easily. To be complete I’d thought I mention those here.

**Possible issue on docker-ce install**![img]()

```
dpkg --configure docker-ce
```
 
**Salt minion doesn’t connect to salt master**

This coud have been our `minion_install.sh` script. We noticed a double entry in `/etc/salt/minion`.

```
vi /etc/salt/minion  # (go to end)

master: salt.toog.io
master: salt.toog.io
```

 **Possible apt list issue**

This most likely works, but you might want to check your “/etc/apt/sources.list” first in combination with how your VPS hosting provider bootstraps your servers.![img]()

```
Do you want to rewrite your 'sources.list' file anyway? If you choose
'Yes' here it will update all 'bionic' to 'focal' entries.
If you select 'No' the upgrade will cancel.
```

 

# References

Some reference we used during our installation:

- Ubuntu upgrade: [How to Upgrade Ubuntu 18.04 LTS to Ubuntu 20.04 LTS – nixCraft](https://www.cyberciti.biz/faq/upgrade-ubuntu-18-04-to-20-04-lts-using-command-line/)
- Docker versioned install: [Install Docker Engine on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)

Good luck.