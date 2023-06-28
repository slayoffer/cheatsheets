### Install/upgrade package

```bash
# to download:
curl https://rpmfind.net/linux/centos/7.9.2009/os/x86_64/Packages/tree-1.6.0-10.el7.x86_64.rpm -o tree-1.6.0-10.el7.x86_64.rpm # -o means to output the file from the link to the filename "tree-1.6.0-10.el7.x86_64.rpm"

# to install:
rpm -ivh /opt/tree-1.6.0-10.el7.x86_64.rpm # -i for install, h for human form, v for verbose
```

### Update package

```bash
rpm -Uvh <rpm-file>
```

### Verify package

```bash
rpm -Vf <rpm-file>
```



### RPM help

```bash
rpm --help
```

### List all the rpm packages

```bash
rpm -qa
# with date
rpm -qa --last
# with date and name
rpm -qa --last postfix3

# Display installed information along with package version and short description
rpm -qi mozilla-mail
```

### Find one specific package

```bash
rpm -qa | grep tree
```

### Erase RPM package

```bash
rpm -ev filename # verbose erasing

# erase without checking for dependencies
rpm -ev --nodeps {package}
```

### Find out what package a file belongs to, i.e. find what package owns the file

```bash
# rpm -qf {/path/to/file}
rpm -qf /etc/passwd
rpm -qf /bin/bash
```

### Display list of configuration file(s) for a package

```bash
rpm -qc httpd
```

### Display list of configuration files for a command

```bash
rpm -qcf /usr/X11R6/bin/xeyes
```

### Display list of all recently installed RPMs

```bash
rpm -qa --last
```

### Find out what dependencies a rpm file has

```bash
rpm -qpR mediawiki-1.4rc1-4.i586.rpm

rpm -qR bash
```

