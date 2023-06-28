### Replace strings in file

```bash
---
- hosts: node01
  become: true
  tasks:
    - replace:
        path: /opt/music/blog.txt
        regexp: 'Kodekloud'
        replace: 'Ansible'

- hosts: node02
  become: true
  tasks:
    - replace:
        path: /opt/music/story.txt
        regexp: 'Ansible'
        replace: 'Kodekloud'
```

