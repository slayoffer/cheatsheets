### Best grep tool

```bash
ripgrep string_name
```

### Cheatsheet

```bash
grep -i pattern file	Case insensitive search
grep -A n pattern file	Show n lines after the match
grep -B n pattern file	Show n lines before the match
grep -C n pattern file	Show n lines before and after the match
grep -v pattern file	Show lines that do not match
grep -c pattern file	Count number of matching lines
grep -l pattern file	Display only the file names
grep -w pattern file	Match the exact word
grep -e regex file	Match the regex pattern
grep -a pattern file	Search into binary files
grep -r pattern dir	Recursively search into directory
```

### Look for a string in the file

```bash
# look for a firewall word (case sensitive)
grep firewall anaconda-ks.cfg

# look for a firewall word (case INsensitive)
grep -i firewall anaconda-ks.cfg
# same
grep -i firewall < anaconda-ks.cfg 

# look through all files in the folder
grep -i firewall *

grep Port /etc/ssh/sshd_config # find Port in sshd config
```

### Search for exact word

```bash
grep -w exam examples.txt
```

### Don't show strings having this line in them (reverse search)

```bash
# no strings with firewall
grep -vi firewall anaconda-ks.cfg # -v for ignore

# check how many active network interfaces pc has
ip a | grep -v LOOPBACK | grep -ic mtu # -i case insensitive, -c count

# get file strings from ls and then get rid off ssh strings which are in file strings
ls | grep file | grep -v ssh 
```

### Show with line numbers

```bash
grep -n Human chars.txt # -n line numbers
```

### Count number of times found

```bash
grep -c "Human" chars.txt # -c count
```

### Find files in the folder

```bash
grep gedit * # looks for gedit in the folder
```

### Show list of files that match the string

```bash
grep -l error *
```

### Search folder and subfolders

```bash
# look through all files in the folder and subfolders, index insensitive
grep -i -r firewall * 
# another example
grep -i -r SELINUX /etc/*
# another
sudo grep -ir 172.16.238.197 /etc > /home/bob/ip.txt # find IP string in /etc dir and redirect to ip file

# Найти все файлы с содержимым site в папке apache2. -ri = (r) поиск по директории и во всех вложенных папках + игнорируя индексацию (i)
grep -riH site /etc/apache2 # -h to show file name
```

### Show with one line below or above

```bash
# find string and show one line below it
grep -A1 arsenal premier-league.txt 

# find string and show two lines above it
grep -B2 arsenal premier-league.txt

# combined
grep -A1 -B2 arsenal premier-league.txt
```

### Regex

```bash
grep 'pattern1\|pattern' filename # \ is needed
```

### Searching multiple patterns at once

```bash
grep -e error -e info access.log
```

### Some examples using grep

```bash
grep red blue.txt # will display lines of text from the blue file that contain the word “red”

rpm –qa | grep smb # will display all installed RPMs with “smb” in their name

# Here is a handy way to use grep. Suppose you need to find a file (or files) containing a particular text string. Use the grep with the –r and –H options to find all files containing that particular string (remember that everything in Linux is case sensitive). By default, grep only prints the text string. If you are looking for files containing the text string, you must tell grep to print the filename, too. The –H command does that.

# In the following statement, -H prints the filename and –r searches recursively from the starting point (lists/) for the text string PHPMAILERHOST:

grep -Hr PHPMAILERHOST lists/

# This is the output from the previous command:
lists/config/config.php:define("PHPMAILERHOST",'');

# You can include the option “n” in your search to display the line number in the file where the string appears.  
```
