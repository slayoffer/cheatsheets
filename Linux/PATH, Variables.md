### System variables

```bash
# show all envs
env

$0 - The name of the Bash script
$1-$9 - The first 9 arguments to the Bash script (As mentioned above)
$# - How many arguments were passed to the Bash script
$@ - All the arguments supplied to the Bash script
$? - The exit status of the most recently run process
$$ - The process ID of the current script
$USER - The username of the user running the script
$HOSTNAME - The hostname of the machine the script is running on
$SECONDS - The number of seconds since the script was started
$RANDOM - Returns a different random number each time is it referred to
$LINENO - Returns the current line number in the Bash script

# example
LOCAL_VAR=A
export GLOBAL_VAR=B
echo $LOCAL_VAR # A
echo $GLOBAL_VAR # B 
bash # starts another shell process
echo $LOCAL_VAR
# nothing
echo $GLOBAL_VAR
# B 

# задавать переменные для текущего сеанса — через set, для текущего и дочерних процессов — export, навсегда и для всех процессов — в файле /etc/profile
```

### Update $PATH variable

```bash
# add dir to $PATH
# create new PATH on the basis of old PATH (in order not to lose all the paths)
# for example add PATH for /bin and /dev dirs
PATH=$PATH:/bin:~/dev

# Java example
sudo PATH=$PATH:/opt/jdk-13.0.2/bin/

# script example
cp ~/hello ~/.local/bin
export PATH=$PATH:$HOME/.local/bin
printenv PATH
/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/home/tux/.local/bin

# to keep the changes after terminal reload add this cmd to .bashrc
# add via if statement
if [ -d ~/bin ]
then
  PATH=$PATH:~/bin
fi
```

### Show variables

```bash
env
```

### Show path variable

```bash
printenv PATH
```

