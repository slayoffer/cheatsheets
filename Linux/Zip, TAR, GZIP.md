

### TAR

```bash
# TAR doesnt compress files, it just archives them!

# create archive
tar -cf etc_backup.tar /etc # -c for create, -f for filename
# for verbose
tar -cfv etc_backup.tar /etc

# to list files inside tar file
tar -tf etc_backup.tar # -t for listing, -f for filename, can add -v for verbose

# to extract
tar -xf etc_backup.tar # -x for extract, -f for filename, can add -v

# extract to folder
sudo tar -C /opt/ -xvf caleston-code.tar.gz
```

### GZIP

```bash
# to compress
gzip picture.img

# to unzip
gunzip picture.img.gz

# with folder
gunzip example.gz -d /folder/example.gz
# or
sudo gunzip -c caleston-code.tar.gz > /opt/caleston-code.tar.gz
```

### Check archive integrity

```bash
# for tar
tar -tf llt-old-backup.tar.gz 1>/dev/null

# for gzip
tar -tzf llt-old-backup.tar.gz 1>/dev/null

# for zip
unzip -t llt-old-backup.tar.zip
```



### Bzip

```bash
# to compress
bzip2 picture.img

# to unzip
bunzip picture.img.bz2
```

### Xz

```bash
# to compress
xz picture.img

# to unzip
unxz picture.img.xz
```

### Compress TAR

```bash
# create tar archive and compress
tar -czf etc-backup.tar.gz /etc # -z for compress tarball (gzip)
# or with verbose
tar -czvf etc-backup.tar.gz /etc

# to view files in archive
tar -tvf etc-backup.tar.gz

# to extract
tar -xvf etc-backup.tar.gz
```

### Read file without uncompress

```bash
bzcat

xzcat

zcat /usr/share/man/man1/tail.1.gz > /home/bob/pipes
```

### Viewing compressed files with zcat

If you use the cat command, you can replace it with `zcat`. zcat is used in exactly the same manner as you use cat. For example:

```
zcat logfile.gz
```

This will display all the contents of logfile.gz without even extracting it.

You can use regular _less_ and _more_ commands with _`zcat`_ to see the output in pages:

```
zcat logfile.gz | less
```

```
zcat logfile.gz | more
```

If you don’t know if the file is compressed (i.e., files without .gz extension), you can use _zcat_ with option _-f_. This will display the content of the file irrespective of whether it is gzipped or not.

```
zcat -f logfile.gz
```

### Reading compressed files with zless and zmore

Same as less and more, you can use `_zless_` and `_zmore_` to read the content of the compressed files without decompressing the files. All the keyboard shortcuts of less and more work the same.

```
zless logfile.gz
```

```
zmore logfile.gz
```

### Searching inside compressed files with zgrep

Grep is a hell of a powerful command and I think, one of the most used Linux commands. `_zgrep_` is the Z counterpart of grep that allows you to search inside gzipped compressed files without extracting them.

You can use it with all the regular grep options. For example:

```
zgrep -i keyword_search logfile.gz
```

### Comparing compressed files with zdiff

While this might not be that useful on huge log files, you can use zdiff to see the difference between compressed files, in the same way as you use the [diff command](https://linuxhandbook.com/diff-command/?ref=itsfoss.com).

```
zdiff logfile1.gz logfile2.gz
```