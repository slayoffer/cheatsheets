### Simple vars

```bash
#!/bin/bash

# assign var
VIRUS="covid19" 

# replace var
myvar=$(( $myvar +1 ))

echo "Due to $VIRUS virus company have lost \$9 million" # => Due to covid19 virus company have lost $9 million

echo "Due to $VIRUS virus company have lost $9 million" # => Due to covid19 virus company have lost million

echo 'Due to $VIRUS virus company have lost $9 million' # => Due to $VIRUS virus company have lost $9 million
```

### Export (ENV) vars

```bash
export SEASON="Monsoon"

# or add to
vim ~/.bash_profile
# to immediately apply all changes to bash_profile, use the source command
source ~/.bash_profile

# delete var from ENV vars but keep in Shell vars
export -n TEST_VAR

# to fully delete var
unset JAVA_HOME

# print all ENV vars
export
# or
export -p
# or
env
# or
printenv

# print only shell vars (non-ENV)
set
# or posix mode
(set -o posix; set)
```

### Automatically export vars during shell session

```bash
set -a # Enable allexport using single letter syntax
set -o allexport # Enable using full option name syntax

set +a # Disable allexport using single letter syntax
set +o allexport # Enable using full option name syntax

# example
set -a
MYVAR=1729    # no export
bash    # Open a new child shell
echo $MYVAR
1729
```

### Save vars from bash script to current shell session

```bash
echo "MYVAR=1729" > myscript.sh
source myscript.sh # need to use source cmd instead of ./myscript.sh to run script
echo $MYVAR
1729
# source cmd doesnt start new shell when executing script
```

### Environment variables

```bash
# check system vars
$ env

$0 - The name of the Bash script
$1-$9 - The first 9 arguments to the Bash script (As mentioned above)
$! - Process ID of the last job run in the background
$# - How many arguments were passed to the Bash script
$@ - All the arguments supplied to the Bash script
$? - The exit status of the most recently run process
$$ - The process ID of the current script
$USER - The username of the user running the script
$HOSTNAME - The hostname of the machine the script is running on
$SECONDS - The number of seconds since the script was started
$RANDOM - Returns a different random number each time is it referred to
$LINENO - Returns the current line number in the Bash 

#!/bin/bash
echo "Name of the script: $0"
echo "Total number of arguments: $#"
echo "Values of all the arguments: $@"
```

### Assign Variable to Command

```bash
UP=`uptime`
echo $UP # will run uptime command

# free memory script
FREE_RAM=`free -m | grep Mem | awk '{print $4}'`
echo "Free RAM is $FREE_RAM mb." # => Free RAM is 370 mb.

# script example

#!/bin/bash
echo "Welcome $USER on $HOSTNAME."
echo "#######################################################"

FREERAM=$(free -m | grep Mem | awk '{print $4}')
LOAD=`uptime | awk '{print $9}'`
ROOTFREE=$(df -h | grep '/dev/sda1' | awk '{print $4}')

echo "#######################################################"
echo "Available free RAM is $FREERAM MB"
echo "#######################################################"
echo "Current Load Average $LOAD"
echo "#######################################################"
echo "Free ROOT partiotion size is $ROOTFREE"
```

### Make var available on reboot

```bash
# for user
vim ~/.bashrc # overrides global vars in /etc/profile

# add line in the end
export SEASON="Monsoon"

# globally for all users
vim /etc/profile

# add line in the end
export SEASON="Monsoon"

# or with custom file
sudo touch /etc/profile.d/http_proxy.sh
sudo vim /etc/profile.d/http_proxy.sh
```
