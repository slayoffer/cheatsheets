### Line

```yaml
---
- hosts: node01
  become: true
  tasks:
  - lineinfile:
      path: /var/www/html/index.html
      line: 'Welcome to KodeKloud Labs!'
      state: present
      insertbefore: BOF # BOF - beginning of the file
```

### Block of text

```yaml
- name: Set Authentication Methods for bender, vagrant, and ubuntu
  blockinfile:
    path: "/etc/ssh/sshd_config"
    block: | # | (pipe) is needed for multi-line entry
      Match User "ubuntu,vagrant"
          AuthenticationMethods publickey
      Match User "bender,!vagrant,!ubuntu"
          AuthenticationMethods publickey,keyboard-interactive
    state: present
  notify: "Restart SSH Server"
```

