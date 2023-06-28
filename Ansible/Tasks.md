### Ansible Tasks

Рассмотрим исходную классическую задачу установки и запуска сервиса на виртуальной машине с помощью Ansible. Базовым структурным блоком в Ansible является задача (`task`). 

`Ansible Task` — это блок в YAML, с помощью которого можно сообщить Ansible, что мы ожидаем получить на целевом сервере: установленный пакет, желаемую конфигурацию и запущенный сервис.

```bash
# В нашей автоматизации список из четырех задач
 
- name: Используем модуль maven_artifact, он скачает пакет приложения с Nexus
  maven_artifact:
    dest: "/opt/sausage-store/backend/lib/sausage-store.jar"
    repository_url: "https://nexus.praktikum-services.ru/repository/sausage-store"
    group_id: "com.yandex.practicum.devops"
    artifact_id: "sausage-store"
    version: "0.1.0"
- name: Добавим сервисного пользователя
  user:
    name: "jarservice"
    create_home: no
    shell: /sbin/nologin
  
- name: Шаблонизация конфига управляет настройками приложения с помощью переменных
  template:
    src: sausage-store-backend.service.j2
    dest: /etc/systemd/system/sausage-store-backend.service

- name: Перечитываем конфигурацию systemd
  systemd:
    daemon_reload: yes

- name: всё готово - запускаем!
  service:
    name: sausage-store-backend
    state: running 
```

Каждой задаче даётся имя, которое будет отображаться в отчёте при запуске Ansible. Далее указывается используемый модуль и его параметры. Параметры модуля хорошо описаны в его документации, которую можно найти на сайте Ansible или вызвав команду `ansible-doc <имя модуля>`