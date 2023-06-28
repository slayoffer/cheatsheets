| yum –help                     | Shows the help                      |
| ----------------------------- | ----------------------------------- |
| yum search PACKAGE            | Search from available repositories  |
| yum install PACKAGE -y        | To install the package              |
| yum install httpd -y          | To install httpd package            |
| yum install vim -y            | To install VIM Editor               |
| yum reinstall PACKAGE         | To reinstall the PACKAGE            |
| Yum remove PACKAGE            | To Remove PACKAGE                   |
| yum upgrade                   | Update all packages                 |
| yum update                    | Update all packages                 |
| yum update PACKAGE            | To Update specific package          |
| yum grouplist                 | List all available Group packages   |
| yum groupinstall “Group Name” | Install all the packages in a group |
| Yum repolist             | List Enabled YUM repositories                                |
| yum install epel-release | Additional package repository that provides easy access to install packages for commonly used software. |
| yum clean all            | Clean yum cache                                              |
| yum history              | View history of yum                                          |
| Yum info PACKAGE NAME    | Shows the information of package like version, size, source, repository etc. |

### Update package registry

```bash
yum check-update
```



### Install

```bash
sudo yum install httpd
```

### Remove

```bash
yum remove httpd
```

### Update

```bash
yum update telnet

# update all packages
yum update
```



### Check package provider

```bash
yum provides httpd
```



### Repos list

```bash
ls /etc/yum.repos.d/
# or
yum repolist

# to see repo URL
cat /etc/yum.repos.d/CentOS-Base.repo

# add new repo
vim /etc/yum.repos.d/nginx.repo
```

### Check package list on remote repo

```bash
yum list ansible

# with different vesrions
yum --show-duplicates list ansible
```

### install specific version

```bash
yum install ansible-2.4.2.0
```

