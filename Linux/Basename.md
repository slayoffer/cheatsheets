## Bash get filename from given path

The basename command strip directory and suffix from filenames. The syntax is pretty simple:
`basename /path/to/filebasename /path/to/file suffix`

### Examples

Let us see how to extract a filename from given file such as /bin/ls. Type the following basename command:
`basename /bin/ls`
You can pass multiple arguments using the -a option as follows:
`basename -a /bin/date /sbin/useradd /nas04/nfs/nixcraft/data.tar.gz`
Store outputs in a shell variable, run:

```
out="$(basename /data/backup-file.tar.gz)"
echo "Filename is $out"
```

### How to remove extension from filenames

Remove .gz from backups.tar.gz file and get backups.tar only:
`basename /backups/14-nov-2019/backups.tar.gz .gz`
OR

```
basename -s .gz /backups/14-nov-2019/backups.tar.gz
#
# Bash get filename from path and store in $my_name variable 
#
my_name="$(basename -s .gz /backups/14-nov-2019/backups.tar.gz)"
echo "Filename without .gz extension : $my_name"
```