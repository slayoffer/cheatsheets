### Linter

```bash
sudo apt install spellcheck

spellcheck script.sh
```

### Arguments

```bash
# scripts examples
echo "Value of 0 is " # only double quotes can be used with vars
echo $0

echo "Value of 1"
echo $1

echo "Value of 2"
echo $2

echo "Value of 3"
echo $3

./args.sh 1 2 3

#
echo "You entered the argument: $1, $2, $3, $4"
./myscript Argument one, two, three, four

#
ls -lh $1
./myscript /etc

#
lines=$(ls -lh $1 | wc -l)

if [ $# -ne 1 ]
then
        echo "This script requires exactly one directory path passed to it."
        echo "Please try again."
        exit 1
fi

echo "You have $(($lines-1)) objects in the $1 directory."
```

### Arrays

```bash
array_name=(value1 value2 value3 … )

files=("f1.txt" "f2.txt" "f3.txt" "f4.txt" "f5.txt")

echo ${files[1]}

# print all files
echo ${files[*]}

# print length of array
echo ${#files[@]}

# update value
files[0]="a.txt"

# append to array
files+=("Kali")

# delete from array
unset files[2]

# delete array
unset files

# different format types array - 4 items
user=("john" 122 "sudo,developers" "bash")
```

"set -e" is a command used in bash scripting that tells the shell to exit immediately if any command returns a non-zero exit status. This is useful for ensuring that your script will not continue executing if an error occurs. For example, if you have a script that requires a certain file to exist, you can use "set -e" to ensure that the script will exit if the file is not found.

```bash
#!/bin/bash

set -e

if [ ! -f "my_file.txt" ]; then
    echo "Error: my_file.txt not found!"
    exit 1
fi
```

### Escaping chars example

```bash
# with escaping
echo 'I\'m 34 years old'
```

### Subshell (make cmd a var)

```bash
files=$(ls) # sends ls to background and creates var
```

### Execute file without X rights

```bash
# show output of the script inside text.txt file without actually executing it
bash text.sh
```

### Give exec rights to script

```bash
chmod +x testscript.sh
```

### Arithmetic, Math

```bash
expr 3 + 3

# multiply
expr 100 \* 4 # needs escaping as * stands for wildcard

# with var
mynum1=100
expr $mynum + 50 # 150

# for floating numbers
echo 10 / 3 | bc -l

# another way
echo $(( A + B ))

# increment, decrement
echo $(( ++A ))
echo $(( A++ ))

# script
num1=$1
num2=$2
num3=$3
sum=$(( num1 + num2 + num3 ))
average=$(echo "$sum / 3" | bc -l)
echo $average
```

### Exit codes

```bash
package=htop

sudo apt install $package >> package_install_results.log

if [ $? -eq 0 ]
then
    echo "The installation of $package was OK."
    echo "The new command is available here:"
    which $package
else
    echo "$package failed to install." >> package_install_failure.log
fi

# forced exit code
directory=/notexist

if [ -d $directory ]
then
    echo "The directory $directory exists"
    exit 0
else
    echo "The directory $directory doesnt exist"
    exit 1
fi

echo "The exit code for this script run is $?"

#
echo "Hello World"
exit 1
echo $? # 1

#
sudo apt install notexist
exit 0
echo $? # 0
```

### Conditions

```bash
[ "abc" = "abc" ]
[ "abc" != "abc" ]
[ 5 -eq 5 ]
[ 5 -ne 5 ]
[ 6 -gt 5 ]
[ 5 -lt 6 ]

# new conditional operator [[ ]] - works in Bash only, can use Regex, etc
[[ "abcd" = *bc* ]] - If abcd contains bc (true)
[[ "abc" = ab[cd] ]] - If 3rd character of abc is c or d (true)
[[ "abc" > "bcd" ]] - If "abc" comes after "bcd" when sorted in alphabetical (lexographical) order (false)
[[ "abc" < "bcd" ]] - If "abc" comes before "bcd" when sorted in alphabetical (lexographical) order (false)

# && ||
[[ A -gt 4 && A -lt 10 ]]
[[ A -gt 4 || A -lt 10 ]]
# or
[ A -gt 4 ] && [ A -lt 10 ]

# file conditions
[ -e FILE ] - file exists
[ -d FILE ] - directory exists
[ -s FILE ] - file exists and size > 0
[ -x FILE ] - file is executable
[ -w FILE ] - file is writable
```



### While loop

```bash
myvar=1

while [ $myvar -le 10 ]
do
   echo $myvar
   myvar=$(( $myvar +1 ))
   sleep 0.5
done

#
while [ -f ~/Scripts/testfile ]
do
  echo "As of $(date), the testfile exists"
  sleep 1
done
echo "As of $(date), the file no longer exists. Exiting."
```





### Read statements (user enter value prompt)

```bash
#!/bin/bash

echo "Enter your skills:"
read SKILL
echo "Your $SKILL skill is in high Demand in the IT Industry."

# another example
read -p 'Username: ' USR # -p for prompt for input
read -sp 'Password: ' pass # -s for supress input (text is not visible, like password, etc)

echo "Login Successfull: Welcome USER $USR,"
```

### If, if else statements

```bash
mynum=200

if [ ! $mynum -eq 200 ]
# or
if [ $mynum -ne 200 ] # -ne for not equal
then
    echo "the condition is true"
else
    echo "This var is not equal 200"
fi

#
read -p "Enter a number: " NUM
echo

if [ $NUM -gt 100 ]
then
   echo "We have entered in IF block."
   sleep 3
   echo "Your Number is greater than 100"
   echo
   date
else
  echo "You have entered number less than 100."
fi

echo "Script execution completed successfully."

#
value=$(ip addr show | grep -v LOOPBACK | grep -ic mtu)

if [ $value -eq 1 ]
then
  echo "1 Active Network Interface found."
elif [ $value -gt 1 ]
then
  echo "Found Multiple active Interface."
else
  echo "No Active interface found."
fi
```

### Check if process is running

```bash
if [ -f /var/run/httpd/httpd.pid ] # -f for file
```

### For loop

```bash
for current_number in 1 2 3 4 5 6 7 8 9 10
do
  echo $current_number
  sleep 1
done

echo "This is outside of the for loop"

#
for (( c=1; c<=5; c++ ))
do  
   echo "Welcome $c times"
done

#
echo "Bash version ${BASH_VERSION}"
for i in {0..10..2}
do     
     echo "Welcome $i times"
done

# tar+gzip example
for file in logfiles/*.log
do
  tar -czvf $file.tar.gz $file
done
```

### Check if file exists

```bash
if [ -f ~/myfile ]
then
    echo "The files exists"
else
    echo "The file does not exist"
fi
```

### Install package

```bash
command=htop

if [ -f $command ] # [] for test
# or
if command -v $command # command checks for existence of cmd
then
    echo "$command is available, lets run it..."
else
    echo "$command is not available, installing it..."
    sudo apt update && sudo apt install $command -y 
fi

$command
```

### Where to store scripts (FHS recommended location)

```bash
/usr/local/bin/
# need to give script ownership to root
sudo chown root:root /usr/local/bin/update.sh
```

### Functions

```bash
# show functions
declare -f | less

# example
check_exit_status() {
   if [ $? -ne 0 ]
   then
       echo "An error occured, please check the $errorlog file."
   fi
}

# remove function from session
unset -f check_exit_status ; check_exit_status
```

