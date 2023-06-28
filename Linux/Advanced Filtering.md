### Step 1) List all packages that start with "linux-"

We'll use the ***dpkg*** command with the ***-l*** switch to list the packages, whether installed or not, that start with the string *linux-*.

dpkg -l linux-*

### Step 2) Filter that list to show only installed packages

To filter the list, I'm going to pipeline the output of the first command into the ***awk\*** command. I'm also going to use ***awk*** to filter out everything but the package names.

dpkg -l linux-* | awk '/^ii/{ print $2 }'

### Step 3) Filter out packages for the currently running kernel

OK, so now I'm down to a pretty limited number of packages, but I don't want to remove the packages for my currently running kernel. I'm going to use a few commands to do that. First off, I can determine my currently running kernel with the ***uname -r*** command. Currently on my system that command outputs: ***2.6.32-25-generic***.

To do my package filtering, I only want the numeric portion of that output. I'll pipeline the output of ***uname -r\*** and use the ***cut*** command with a hyphen as the field delimiter. I'll cut fields 1 & 2.

uname -r | cut -f1,2 -d"-"

Now I'm going to use this result as the filter for a ***grep*** command. In Linux, to use the result of one command as an argument in another command, you enclose the command in single back-quotes ( ` that's the key to the left of the 1 on a standard US keyboard). So here's my one-liner so far.

dpkg -l linux-* | awk '/^ii/{ print $2}' | grep -v -e `uname -r | cut -f1,2 -d"-"`

This is the output so far on my system:

linux-firmware
linux-generic
linux-headers-2.6.32-24
linux-headers-2.6.32-24-generic
linux-headers-generic
linux-image-2.6.32-24-generic
linux-image-generic
linux-libc-dev
linux-sound-base

### Step 4) Filter the list for only the kernel packages

So now I have a package list that excludes the packages for my current kernel. The only packages from the list above that I want to remove are: **linux-headers-2.6.32-24, linux-headers-2.6.32-24-generic, linux-image-2.6.32-24-generic**.

What makes these packages unique from the others in the list is that they all contain numbers. So I can use ***grep*** again to filter the list down to only packages with numbers in their names. I'll pipeline the output of the previous command into ***grep -e [0-9]\***.

dpkg -l linux-* | awk '/^ii/{ print $2}' | grep -v -e `uname -r | cut -f1,2 -d"-"` | grep -e [0-9]

So now the output on my system is only the following:

linux-headers-2.6.32-24
linux-headers-2.6.32-24-generic
linux-image-2.6.32-24-generic

### Step 5) Make sure we don't catch any stray packages

Some people have had problems with this one-liner catching the ***linux-libc-dev:amd64*** package. Adding a

grep -E "(image|headers)"

to the string fixes the problem. Now we have

dpkg -l linux-* | awk '/^ii/{ print $2}' | grep -v -e `uname -r | cut -f1,2 -d"-"` | grep -e [0-9] | grep -E "(image|headers)"

### Step 6) Putting it all together: Removing the packages

So now that I have a good list of packages I can use another pipe and the ***xargs*** command to invoke ***apt-get\*** to remove the packages. First I'm going to show it using the ***--dry-run\*** switch with ***apt-get\***. That way you can give it a try without actually changing your system.

dpkg -l linux-* | awk '/^ii/{ print $2}' | grep -v -e `uname -r | cut -f1,2 -d"-"` | grep -e [0-9] | grep -E "(image|headers)" | xargs sudo apt-get --dry-run remove

If everything looks good after the dry run, you can go ahead and remove the old kernels with:

dpkg -l linux-* | awk '/^ii/{ print $2}' | grep -v -e `uname -r | cut -f1,2 -d"-"` | grep -e [0-9] | grep -E "(image|headers)" | xargs sudo apt-get -y purge

So there you have it. One command, albeit a long one, to remove the old kernels from your Ubuntu system. I imagine the same command should work on other Debian based systems as well, but I've only tested this on my 32 bit system. I'd be interested to know if it works just as well on a 64 bit system.