### Create multiple files with similar names

Create files with a particular name pattern:

```
abhishek@LHB:~/test$ touch file_{1..10}.txt
abhishek@LHB:~/test$ ls
file_10.txt  file_2.txt  file_4.txt  file_6.txt  file_8.txt
file_1.txt   file_3.txt  file_5.txt  file_7.txt  file_9.txt
```

### Create backup file

When you are about to edit a config file, it is recommended to make a backup. The general convention is to add a `.bak` extension to the original filename. This signifies that it's a backup of the given filename.

```
cp -p long_filename.txt long_filename.txt.bak
```

That's cool but let's use the brace expansion:

```
cp -p long_filename.txt{,.bak}
```

Yupe! This `{,text}` is not the usual `{X..Y}` pattern but it's a useful one you should know.

The `-p` option of the [cp command](https://linuxhandbook.com/cp-command/) preserves the file properties like ownership, [timestamps](https://linuxhandbook.com/file-timestamps/), etc.

### Use multiple braces

You can use multiple braces to create files with similar names and different extensions. That's only one example of using multiple braces.

```
abhishek@LHB:~/test$ touch {a,b,c}.{hpp,cpp}
abhishek@LHB:~/test$ ls
a.cpp  a.hpp  b.cpp  b.hpp  c.cpp  c.hpp
abhishek@LHB:~/test$
```

### Using brace expansion in path

Let's say you have a similar directory structure with only a slight change. Brace expansion can be helpful here.

```
mv project/{new,old}/dir/file
```

The above command is equivalent to:

```
mv project/new/dir/file project/old/dir/file
```

## Not everything can be expanded

That goes without saying. You are looking to create sequences so it should be something that could be created into sequences. If you use a weird combination, it won't be expanded.

```
abhishek@LHB:~$ echo {1..Z}
{1..Z}
```

You cannot use decimal points.

```
abhishek@LHB:~$ echo {1..5..0.5}
{1..5..0.5}
```

It may generate weird results for some weird combinations:

```
abhishek@LHB:~$ echo {a..F}
a ` _ ^ ]  [ Z Y X W V U T S R Q P O N M L K J I H G F
```