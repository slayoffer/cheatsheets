### Old way (inventory.txt)

```bash
# Web Servers
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

# Database Servers
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_password=Password123!

[all_servers:children]
web_servers
db_servers

[web_servers]
web1
web2
web3

[db_servers]
db1
```

### Good way (inventory.yaml)

В списке могут быть DNS-имена или IP-адреса:

**hosts.txt**

```bash
localhost
10.66.101.8
sausage-backend 
```

Серверов, на которых нужно выполнить один play, может быть несколько, поэтому хосты организуют в группы. Группы могут содержать список хостов или вложенные группы, а **inventory** тогда удобнее описать в том же YAML:

**inventory.yaml**

```bash
all: # Все серверы в нашем inventory, all - обязателен
  vars: # vars for all hosts
  	ansible_ssh_private_key_file: ~/.ssh/id_rsa
	ansible user: ansible
  children: # Дочерние группы для all
    backend: # Группа хостов с именем backend
      hosts:
        10.66.101.8:
    frontend:
      hosts:
        sausage-frontend.example.com: 
```

Такая вложенность групп позволяет разбить инфраструктуру на окружения (`dev`/`stage`/`prod`):

```bash
all: # Все серверы в нашем inventory, all - обязателен
  children:
    backend: # Группа хостов с именем backend
      hosts:
        10.66.101.8:
        dev-backend.example.com:
    frontend:
      hosts:
        10.66.101.7:
        sausage-frontend.example.com:
    dev:
      hosts:
        dev-backend.example.com:
        10.66.101.7:
    prod:
      hosts:
        10.66.101.8:
        sausage-frontend.example.com:   
```

И запускать плейбук с параметром `--limit`, только по нужному окружению:

```bash
ansible-playbook playbook.yaml --limit dev 
```

В **inventory** можно описать переменные группы или хоста, которые можно будет использовать в задачах:

```bash
all:
  children:
    backend:
      hosts:
        dev-backend.example.com:
          # Переменная только для этого хоста
          ansible_user: student
      vars: # Переменные для группы backend
        backend_version: 0.1.0
  vars: # Переменные для всех хостов в inventory
    ansible_connection: ssh 
```

Но такой формат передачи переменных перегружает файл **inventory** и лучшей практикой будет расположить переменные групп в директории `group_vars` рядом с файлом плейбука:

```bash
$ tree -L 2 .
.
├── group_vars
│   ├── all.yml
│   ├── backend.yml
│   └── frontend.yml
└── playbook.yaml 
```

Пример содержимого файла `group_vars/all.yml`:

```bash
ansible_connection: ssh 
```

В современном мире инфраструктура может быть очень динамичной, поэтому чаще всего используется динамический **inventory**. Ansible поддерживает эту функциональность в виде плагинов динамического инвентаря — такой плагин подключается к облачной платформе или API системы виртуализации, получает список серверов и передаёт его в Ansible.