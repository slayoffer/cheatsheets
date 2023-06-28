### How to update grub

Ubuntu and many other Linux distributions provide a handy command line utility called update-grub.

To update grub, all you have to do is to run this command in the terminal with sudo.

```bash
sudo update-grub
```

You should see an output like this:

```bash
Sourcing file `/etc/default/grub'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-5.0.0-37-generic
Found initrd image: /boot/initrd.img-5.0.0-37-generic
Found linux image: /boot/vmlinuz-5.0.0-36-generic
Found initrd image: /boot/initrd.img-5.0.0-36-generic
Found linux image: /boot/vmlinuz-5.0.0-31-generic
Found initrd image: /boot/initrd.img-5.0.0-31-generic
Found Ubuntu 19.10 (19.10) on /dev/sda4
Found MX 19 patito feo (19) on /dev/sdb1
Adding boot menu entry for EFI firmware configuration
done
```

You may see a similar command called update-grub2. No need to be alarmed or confused between update-grub and update-grub2. Both of these commands do the same action.

Around ten years ago, when grub2 was just introduced, update-grub2 command was also introduced. Today, update-grub2 is just a symbolic link to update-grub and both update grub2 configuration (because grub2 is the default).