### Vars priority checking

```bash
# from least to highest priority
1 command line values (for example, —u my_user, these are not variables)
2 role defaults (defined in role/defaults/main.yml)
3 inventory file or script group vars
4 inventory group_vars/all
5 playbook group_vars/all
6 inventory group_vars/*
7 playbook group_vars/*
8 inventory file or script host vars
9 inventory host_vars/*
10 playbook host_vars/*
11 host facts / cached set_facts
12 play vars
13 play vars_prompt
14 play vars_files
15 role vars (defined in role/vars/main.yml)
16 block vars (only for tasks in block)
17 task vars (only for the task)
18 include_vars
19 set_facts / registered vars
20 role (and include_role) params
21 include params
22 extra vars (for example, —e "user=my_user")
```

### Var syntax

```yaml
# inventory.txt
worker-1 ansible_host=192.168.1.106 ansible_ssh_pass=osboxes.org ansible_connection=ssh ansible_port=22

# inventory.yaml
-
    name: 'Update nameserver entry into resolv.conf file on localhost'
    hosts: localhost
    vars:
        car_model: BMW M3
        country_name: USA
        title: Systems Engineer
    tasks:
        -
            name: 'Print my car model'
            command: 'echo "My car''s model is {{ car_model }}"'
        -
            name: 'Print my country'
            command: 'echo "I live in the {{ country_name }}"'
        -
            name: 'Print my title'
            command: 'echo "I work as a {{ title }}"'
```

### Hosts vars file

```bash
# define file with the host name
vim web01.yml
# add vars
dns_server: 10.0.0.5
```

### Test vars

```bash
ansible -m debug -a 'var=hostvars[inventory_hostname]' database # database is var file name
```



В Ansible переменные используются для управления поведением задач так же, как в программировании параметры функции управляют результатом выполнения этой функции. Переменные могут быть определены в Ansible-плейбуках, в Ansible-**inventory**, внутри Ansible-ролей или передаваться в командной строке запуска плейбука. В последнем случае такие переменные называются «внешними» или «extra vars»:

```bash
ansible-playbook playbook.yaml --extra-vars "my_var=value" 
```

Внешние переменные имеют наивысший приоритет. Например, если переменная `my_var` определена в файле `group_vars/<my group>.yaml`, то запуск плейбука с параметром `--extra-vars "my_var=value"` заменит значение этой переменной.

В то же время, переменные, указанные в файлах `group_vars/`, имеют больший приоритет над переменными, заданными в Ansible-ролях.

Ansible даёт широкие и гибкие возможности параметризации, но эта гибкость может усложнить проект при злоупотреблении. Расположение переменных зависит от решаемой задачи, а для нашего проекта мы покажем пример организации файлов в конце урока.

### Jinja2 vars

Ansible использует движок шаблонизации Jinja2 для подстановки переменных или для динамического формирования конфигурационных файлов.

В любом YAML-блоке Ansible можно указать переменную в двойных фигурных скобках и Jinja2 подставит соответствующее значение.

Например, ссылка одной переменной на значение другой в `group_vars/all.yml`:

```bash
answer: 42
question: "{{ answer }}" 
```

Или в описании задачи:

```bash
- name: Install package {{ package_name }}
  package:
    name: "{{ package_name }}" 
```

Ansible-модуль **template** использует Jinja2 для создания файлов на основе шаблонов с помощью переменных в процессе выполнения:

```bash
- name: Ensure Sausage Store Backend service unit 
  template:
    src: sausage-store-backend.service.j2
    dest: /etc/systemd/system/sausage-store-backend.service 
```

Если мы опишем переменные бэкенда в `group_vars/backend.yml`:

```bash
backend_maven_version: 0.1.0
backend_service_user: jarservice
backend_report_directory: /opt/sausage-store/reports
backend_lib_directory: /opt/sausage-store/lib/ 
```

То сможем генерить systemd-юнит файл на основе шаблона `sausage-store-backend.service.j2`:

```bash
[Unit]
Description=sausage-store

[Service]
User={{ backend_service_user }}

Restart=always

Environment=REPORT_PATH={{ backend_report_directory }}

ExecStart=/usr/bin/java -Xrs -jar {{ backend_lib_directory }}/sausage-store-{{ backend_maven_version }}.jar

[Install]
WantedBy=multi-user.target 
```

Сохранить в переменную `memory_free` количество мегабайт свободной памяти на хосте:

```bash
memory_free: "{{ ansible_facts['ansible_memfree_mb'] }}"
```

### !!! DONT USE ROLE DIR /vars to store vars !!!
