### Update package index (apt update)

```bash
ansible all -m apt -a update_cache=true --become --ask-become-pass # will ask for user passwd
```

### Upgrade dist

```bash
ansible all -m apt -a upgrade=dist --become --ask-become-pass
```

### Install app

```bash
ansible all -m apt -a name=vim-nox --become --ask-become-pass

# latest version
ansible all -m apt -a "name=snapd state=latest" --become --ask-become-pass
```

### Copy (SCP)

```bash
ansible localhost -m copy -a "src=sausage-store-backend.service dest=/tmp/sausage-store-backend.service"
```
