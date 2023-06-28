## Show iNodes

```bash
# iNodes contain meta data about files and folders
ls -i # -i for inodes
# or
ls -li
```

### Create a soft/symbolic link

```bash
# soft link for /etc/zabbix/php-fpm.conf file with name zabbix-php-fpm.conf
sudo ln -s /etc/zabbix/php-fpm.conf zabbix-php-fpm.conf

# soft links have different inodes than original files
ln -s /home/slayo/Documents/weekly_report.txt /home/slayo/Desktop # link file to desktop (shortcut)

# if outside the folder of needed file creation only works with full path, not relative!
# if making relative path then can only create link in same dir
ln -s Documents/weekly_report.txt relative_link

# this will not work unless cd into /home/slayo/Desktop/
ln -s Documents/weekly_report.txt /home/slayo/Desktop/relative_link # will create invalid link

# to be able to move soft link need to include full path when creating
# soft link can be moved to other file system (usb, SMB, etc)
```

### Create hard link

```bash
ln /home/slayo/Documents/weekly_report.txt /home/slayo/Desktop/new_link
# hard link is a clone entry of original file
# hard link can be used with relative path
# original file can be renamed, but hard link will still work
# hard links have same inode numbers as original files
# hard links CANT be moved to other file systems
# its tough to differentiate hard link from file, better try not to use it
```

### Unlink

```bash
unlink <link name>
```

## Find broken symbolic links

If we want to find all the broken symbolic links on our system, we can run the following command:

```
$ find . -type l ! -exec test -e {} \; -print
```
