### To copy a file from one location to another:

```bash
cp /path/to/source/file /path/to/destination/directory/
```

### To copy a file and rename it:

```bash
cp /path/to/source/file /path/to/destination/directory/newfile
```

### Copy multiple files to another location

To copy multiple files to another directory, execute the command in the following fashion:

```bash
cp File1 File2 File3 FileN Target_directory
```

### To copy a directory and its contents recursively:

```bash
cp -r /path/to/source/directory /path/to/destination/directory/
```

### Deal with duplicate files while copying

By default, the cp command will override the file if a file with the same name exists in the target directory.

To avoid overriding, you can use the `-n` option with the cp command, and it won't override the existing files:

```bash
cp -n Source_File Destination_directory
```

For example, here, I have tried to copy two files that were already there in my targeted directory and used `-v` option to showcase what is being done by the command:

```bash
cp -n -v itsFOSS.txt LHB.txt LU.txt ~/Tux
```

### To preserve the original timestamp and permission of a file while copying:

```bash
cp -p /path/to/source/file /path/to/destination/directory/
```

### To force the copy of a file, even if the destination file exists and is write-protected:

```bash
cp -f /path/to/source/file /path/to/destination/directory/
```

Note: The exact syntax and behavior of the `cp` command may vary depending on the operating system and shell being used.

### To copy only the files in a directory

You can use the `*` wildcard character with the `cp` command. The `*` will match any file in the directory, but not the subdirectories.

For example, to copy all files in the `source/directory` to the `destination/directory`, you can use the following command:

```bash
cp /path/to/source/directory/* /path/to/destination/directory/
```

This will copy all files in `source/directory` to the `destination/directory`. If you want to copy only certain types of files, you can use a wildcard pattern that matches those files. For example, to copy only text files with a `.txt` extension, you can use:

```bash
cp /path/to/source/directory/*.txt /path/to/destination/directory/
```

This will copy all files in `source/directory` with a `.txt` extension to the `destination/directory`.

Or to copy only the contents of the directory, not the directory itself, you append `/.` at the end of the source directory's name:

```bash
cp -r Source_directory/. Destination_directory
```

### Interactively copy files

But what about when you want to override some files, whereas some should be kept intact?

Well, you can use the cp command in the interactive mode using the `-i` option, and it will ask you each time whether the file should be overridden or not:

```bash
cp -i Source_file Destination_directory
```

### Copy multiple directories

To copy multiple directories, you will have to execute the command in the following way:

```bash
cp -r Dir1 Dir2 Dir3 DirN Destiniation_directory
```

You can do the same when you want to copy files from multiple directories but not the directory itself:

```bash
cp -r Dir1/. Dir2/. Dir3/. DirN/. Destination_directory
```

### cp -a

The `cp -a` command is used to copy files and directories while preserving their permissions, ownership, timestamps, and other attributes. The `-a` option is a shorthand for the following options:

- `-p`: Preserve file timestamps and ownership information.
- `-r`: Copy directories and their contents recursively.
- `-l`: Create hard links instead of copying files if possible.
- `-d`: Preserve symbolic links instead of following them.

So the `cp -a` command is a convenient way to copy files and directories while preserving all of their attributes. This is particularly useful when you need to create a backup or make a copy of a directory hierarchy without losing any information.

For example, to copy the `source/directory` to the `destination/directory` while preserving all file attributes, you can use the following command:

```bash
cp -a /path/to/source/directory /path/to/destination/directory/
```

Note that the `-a` option is not available on all versions of the `cp` command, and may be replaced with different options or flags on some systems.