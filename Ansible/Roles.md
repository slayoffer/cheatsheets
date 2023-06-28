### Create role dir with files

```bash
ansible-galaxy init role_name # will create dirs and files for this role
```

### Default roles dir

```bash
/etc/ansible/roles

# check roles path in config
ansible-config dump | grep ROLE

# change roles path
vim /etc/ansible/ansible.cfg
roles_path = /...
```

### Use roles from Galaxy

```bash
# search role on Galaxy
ansible-galaxy search mysql
# install
ansible-galaxy install role_name
# or with file name
ansible-galaxy install -r requirements.yml
# or with dir
ansible-galaxy install role_name -p ./roles

# list installed roles
ansible-galaxy list
```

### Add role to playbook

```bash
name: Install and Configure MYSQL
hosts: db-server
 roles:
  - role: geerlingguy.mysql
    become: yes # for sudo access
  - role: some_other_role
    become: no
    vars:
      mysql_user_name: db-user
```



### Ansible Roles

Когда задач становится много или когда нужно управлять таким набором задач как единым модулем, они организуются в Ansible-роли (`roles`). Ansible-роль инкапсулирует всю сложную логику, как функция в программировании, и позволяет вызывать код этой логики одной строкой с параметрами.

Ansible-роль представляет собой директорию, с упорядоченными по назначению YAML-файлами.

Файловая структура роли чётко определена:

`tasks/main.yml` — Ansible-задачи, запускаемые ролью. 

`defaults/main.yml` — дефолтные значения переменных для роли.

`vars/main.yml` — переменные для роли.

`templates/` — директория с jinja2-шаблонами.

`handlers/main.yml` — обработчики, запускаются только при получении соответствующих уведомлений (**notify**) от задач.

`files/` — директория с файлами, не требующими шаблонизации.

`meta/main.yml` — метаданные с описанием авторов роли, зависимостей и версии.

Файловая структура роли может быть автоматически сгенерирована с помощью утилиты `ansible-galaxy`, которая поставляется в пакете Ansible