You can install a `.deb` package in Ubuntu by using the `dpkg` command. Here's how to do it:

First, you need to navigate to the directory where your `.deb` file is located. You can do this using the `cd` command:

```bash
cd /path/to/directory
```

Then you can use the `dpkg` command to install the package:

```bash
sudo dpkg -i plexmediaserver_1.32.4.7195-7c8f9d3b6_amd64.deb
```

Here, `-i` stands for 'install'.

Please note that sometimes, the package might have some unmet dependencies. If that's the case, you will get an error about these dependencies. In such situations, you can use the `apt-get` command with `-f` option to fix the broken dependencies:

```bash
sudo apt-get install -f
```

This command will resolve and install any unmet dependencies on your system.

Remember to replace "/path/to/directory" with the path where your `.deb` file is actually located. For instance, if the file is in your Downloads directory, the command will look like this:

```bash
cd ~/Downloads
```