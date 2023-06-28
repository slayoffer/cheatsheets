Команда sestatus
Обычно SELinux (модуль безопасности Linux) подключается там, где
запущены процессы на хосте, требуются наименьшие привилегии,
предотвращая тем самым появление наиболее часто возникающих
процессов, связанных с важными процессами в системе. Однако в некоторых
приложениях требуется доступ к старшему файлу, который может выдать
ошибку. Чтобы проверить, не блокирует ли SEL в сообщениях приложения,
связать команду tail и grepзапросить запрос типа «denied» в лог-файле
/var/log/audit .
Проверка того, включен ли сам модуль SELinux, созданный с помощью
команды sestatus :
$ sestatus
SELinux status: enabled
SELinuxfs mount: /sys/fs/selinux
SELinux root directory: /etc/selinux
Loaded policy name: targeted
Current mode: enforcing
Mode from config file: enforcing
Policy MLS status: enabled
Policy deny_unknown status: allowed
Max kernel policy version: 28
Вывод предложений на то, что на хосте включен модуль SELinux.  