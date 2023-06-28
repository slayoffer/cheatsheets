### Du

1. To get the size of a folder in bytes:

```bash
du -b /path/to/folder
```

1. To get the size of a folder in kilobytes:

```bash
du -h -s /path/to/folder
```

1. To get the size of a folder in megabytes:

```bash
du -h -s --block-size=1M /path/to/folder
```

1. To get a list of all subfolders and their sizes within a folder:

```bash
du -h /path/to/folder/*
```

In all of the above examples, replace `/path/to/folder` with the actual path to the folder you want to check. The `-h` option is used to display the size in a human-readable format, and the `-s` option is used to suppress the display of individual sizes of each file or subdirectory within the folder.

### Ncdu

1. Install `ncdu` if it's not already installed on your system. You can do this using your system's package manager. For example, on Ubuntu, you can run:

```bash
sudo apt install ncdu
```

1. Once installed, you can run `ncdu` and specify the path to the folder you want to check:

```bash
ncdu /path/to/folder
```

1. `ncdu` will start scanning the folder and display the folder size along with a list of subfolders and files. You can navigate through the folder structure using the arrow keys and enter key.
2. `ncdu` provides a lot of options for filtering and sorting the displayed results. For example, you can use the `-r` option to display files and subfolders in reverse order, the `-x` option to exclude certain files or folders, and the `-o` option to save the scan results to a file. For a full list of options, you can refer to the `ncdu` manual by running `man ncdu`.