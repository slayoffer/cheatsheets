### Создать пустой файл

```bash
touch text.txt

# create without updating timestamp in case file exists
touch -c text.txt

# add text to file after creating without editor
cat > newfile.txt
```

### Delete contents of file

```bash
truncate -s 0 access.log

# or
> access.log

# or
: > access.log

# or
true > access.log

# or
cat /dev/null > access.log

# or
cp /dev/null access.log

# or
dd if=/dev/null of=access.log

# or
echo "" > access.log

# or
echo > access.log

# or
echo -n "" > access.log
```

### Чтобы получить дату создания файла и iNode:

```bash
# get inode
ls -i /dir/to/some/file.txt

# А для просмотра файловой системы:
df /dir/to/some/file.txt

# Теперь чтобы получить дату создания файла, используйте команду:
sudo debugfs -R 'stat <inode>' /file/system

# or
stat /dir/to/some/file.txt
# or
statx /dir/to/some/file.txt
```

## Share a folder quickly using a web server

Using python, we can easily share a folder in a URL.

```
cd $mydir && python3 -m http.server 8888
```

## Count files in the current directory, including subdirectories

```
find . -type f | wc -l
```

## Remove executable bit

The following command recursively removes the executable bit from all files in the current directory.

```
chmod -R -x+X *
```

### Delete many files

```bash
yes | rm -i * # will auto put -y to each file
```

### Securely deletes a file

```bash
shred zvu file_name
```

### Check file type

```bash
file text.txt
```

### **Create several files with script**

```bash
# create ten files with 1-10 numbers
touch devopsfile{1..10}.txt
```

### **Move and rename files & dirs**

```bash
# files
mv devopsfile4.txt ops/

# dirs
mv ops dev

# move all the files with .txt format
mv *.txt textdir/

# rename file
mv testfile1.txt testfile22.txt

# move several
mv dir1 dir2 dir3 /path/to/move
```

### Remove files & dir

```bash
rm devopsfile2.txt

# remove dir
rm -r mobile

# remove everything in the folder with confirmation check ("-i")
rm -ri *

# remove all without confirm check ("-f")
rm -rf *

# remove and print what its doing ("-v")
rm -rv
```

### Count the lines in the file

```bash
wc -l /etc/passwd
# or
wc -l < /etc/passwd

# script
#!/bin/bash
nlines=$(wc -l < $1)
echo "There are $nlines lines in $1"
```

### Create dir with sub dirs

```bash
mkdir -p /opt/dev/ops/devops/test
# or
mkdir --parent /opt/dev/ops/devops/test
# with verbose
mkdir -pv /opt/dev/ops/devops/test
```