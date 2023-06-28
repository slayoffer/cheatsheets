### Package

```yaml
- name: Install ntpdate
  ansible.builtin.package:
    name: ntpdate
    state: present

# This uses a variable as this changes per distribution.
- name: Remove the apache package
  ansible.builtin.package:
    name: "{{ apache }}"
    state: absent

- name: Install the latest version of Apache and MariaDB
  ansible.builtin.package:
    name:
      - httpd
      - mariadb-server
    state: latest
```

### Apt

```yaml
- name: Install python3-flask, gunicorn3, and nginx
  apt:
    name:
      - python3-flask
      - gunicorn
      - nginx
    update_cache: yes
```



### Yum

```yaml
---
- name: 'Install the required package'
  hosts: localhost
  become: yes
  connection: local
  tasks:
    - yum: 
        name: vim-enhanced
        state: present
```

