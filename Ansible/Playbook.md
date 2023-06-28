### Samples

```yaml
---
- name: Execute a command on localhost
  hosts: localhost
  connection: local
    tasks:
        - name: Execute a command
      	  command: cat /etc/resolv.conf
      	  
        - name: 'Add entry line into /etc/resolv.conf'
          lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver 10.1.250.10'

        - name: 'create a new web user'
           user:
                name: johnd
                uid: 1040
                group: developers

        - name: 'Execute a script'
           script: /tmp/install_script.sh

        - name: 'Start httpd service'
           service:
                name: httpd
                state: present
                
# run
ansible-playbook -i localhost, command.yml -vv # -vv for verbose mode
```



### Start playbook

```bash
ansible-playbook --ask-become-pass install_apache.yml

# if remote_user is added to ansible.cfg
ansible-playbook site.yml
```

### Ansible Playbook

Клеем, связывающим код ролей или задач Ansible с целевыми серверами, является плей (`play`). 

`Ansible Play` — это блок описания в YAML, в котором указан шаблон целевых хостов (или ещё говорят `host pattern`) и список задач или ролей для этих хостов.

```bash
---
- name: Один плей для запуска backend сервиса сосисочной
  hosts: backend # Шаблон целевых хостов это группа хостов с именем backend
  roles: # Список ansible-ролей для backend-серверов
    - sausage-store-backend 
```

Список плеев организуется в Ansible-плейбук (`playbook`) — YAML-файл, который передаётся утилите `ansible-playbook`:

**playbook.yaml**

```bash
---
- name: Один плей для запуска backend сервиса сосисочной
  # Шаблон целевых хостов это группа хостов с именем backend
  hosts: backend
  # Список ansible-ролей для backend-серверов
  roles:
    - sausage-store-backend

- name: Второй плей для запуска frontend сервиса сосисочной
  # Шаблон целевых хостов это группа хостов с именем frontend
  hosts: frontend
  # Список ansible-ролей для frontend-серверов
  roles:
    - sausage-store-frontend 
```

Запуск такого плейбука будет выглядеть следующим образом:

```bash
ansible-playbook playbook.yaml 
```

Ansible последовательно выполнит один плей за другим, запуская задачи ролей на указанных хостах.
