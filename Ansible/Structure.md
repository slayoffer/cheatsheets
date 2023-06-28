### Структура проекта Ansible

Соберём все части пазла вместе. Для нашей сосисочной Ansible-проект может выглядеть следующим образом:

```bash
$ tree -L 2
.
├── README.md
├── ansible.cfg
├── roles
│   ├── backend
│   └── frontend
├── group_vars
│   ├── all.yaml
│   ├── backend.yaml
│   └── frontend.yaml
├── inventory
│   ├── static.yaml
│   └── yandexcloud.yaml
└── playbook.yaml 
```