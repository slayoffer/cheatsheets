### To disable GRUB waiting on boot

```bash
# edit config file
vim /etc/default/grub

# change
GRUB_HIDDEN_TIMEOUT=0
GRUB_HIDDEN_TIMEOUT_QUIET=true

# apply
sudo update-grub
```

