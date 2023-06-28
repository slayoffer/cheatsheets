### Permissions

```bash
---
- hosts: all
  become: true
  tasks:
    - name: Copy file with owner and permissions on node01
      copy:
        src: /usr/src/condition/blog.txt
        dest: /opt/condition/blog.txt
        owner: bob
        group: bob
        mode: "0640"
      when: inventory_hostname == "node01"

    - name: Copy file with owner and permissions on node02
      copy:
        src: /usr/src/condition/story.txt
        dest: /opt/condition/story.txt
        owner: sam
        group: sam
        mode: "0400"
      when: inventory_hostname == "node02"
```

### Group

```bash
- name: Assign 'bender' to the 'developers' group
  user:
    name: bender
    groups: developers
    append: yes # if set to -no- or omitted it will remove user from all other groups except this one
```

### Sudo

```bash
# add and check sudoers file for errors
- name: Create sudoers file for developers group
  template:
    src: "../ansible/templates/developers.j2"
    dest: "/etc/sudoers.d/developers"
    validate: 'visudo -cf %s' # %s for /etc/sudoers.d/developers, -cf for check syntax errors
    owner: root
    group: root
    mode: 0440
```

