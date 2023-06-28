| DNF commands cheatsheet | https://www.linuxtechi .com/dnf-command examples-rpm management-fedora linux/ |
| ----------------------- | ------------------------------------------------------------ |
| dnf --help              | Show the help                                                |
| dnf search PACKAGE     | search from available repositories |
| dnf install PACKAGE -y | To install the package             |
| dnf install httpd -y   | To Install httpd package           |
| dnf install vim -y     | Installing VIM Editor              |
| dnf reinstall PACKAGE  | To reinstall PACKAGE               |
| dnf remove PACKAGE     | To remove PACKAGE                  |
| dnf update             | update all packages                |
| dnf update PACKAGE     | update only a package              |
| dnf grouplist                | List all available Group Packages                            |
| dnf groupinstall "GROUPNAME" | Installs all the packages in a group                         |
| dnf repolist                 | List Enabled dnf Repositories                                |
| dnf clean all                | Clean dnf Cache                                              |
| dnf install epel release     | Additional package repository that provides easy access to install packages for commonly used software. |
| dnf history           | View History of dnf                                          |
| dnf info package name | Shows the information of package like version, size, source, repository etc |

## **Installation of DNF in RHEL/CentOS 7**

\1) To install DNF on RHEL/CentOS 7 systems, you need to set up and enable epel YUM REPO before installing DNF.

> \# yum install epel-release

\2) Install DNF

> \# yum install DNF

\3) You can now start to run commands using DNF. To view the man page you can use the following command:

> \# dnf –help

 

**DNF vs YUM command Examples**

**1) Install a Package**

> YUM: yum install <package>

Eg: yum install httpd

> DNF: dnf install <package>

Eg: dnf install httpd

 

**2) Upgrade a package**

> YUM: yum upgrade <package>

Eg: yum update mysql-server

> DNF: dnf upgrade <package>

Eg: dnf upgrade mysql-server

 

**3) Remove a package**

> YUM: yum remove <package>

Eg: yum remove httpd

> DNF: dnf remove <package>

Eg: dnf remove httpd

 

**Advantages of DNF**

\1) DNF comes with a simplified code: DNF has about 29000 lines of code compared to over 59000 in yum.

\2) Support for multiple repositories.

\3) Faster and lesser memory intensive operations compared to yum.

\4) Simple interface.

\5) DNF runs in both Python 2 and Python 3.

\6) Simple configuration.

\7) DNF has faster dependency resolving speed than yum.

\8) RPM consistent behavior.

\9) C bindings for lower level libraries.

\10) Package group support, including multiple-repository groups.

The default location of DNF configuration file is /etc/dnf/dnf.conf.

 

**Available DNF commands**

autoremove

check-update

clean

distro-sync

downgrade

group

help

history

info

install

list

makecache

mark

provides

reinstall

remove

repolist

repository-packages

search

updateinfo

upgrade

upgrade-to