### List the available tags in a playbook

```bash
ansible-playbook --list-tags site.yml
```

### Examples of running a playbook but targeting specific tags

```bash
ansible-playbook --tags db --ask-become-pass site.yml
 
ansible-playbook --tags centos --ask-become-pass site.yml
 
ansible-playbook --tags apache --ask-become-pass site.yml

# for multiple tags
ansible-playbook --tags "apache,db" --ask-become-pass site.yml
```

