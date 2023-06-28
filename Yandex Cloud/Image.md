### Перенос локальной ВМ в Compute Cloud

Если вы разрабатываете сервис на рабочей станции в локальной ВМ, то можете перенести машину в Compute Cloud. Для этого [подготовьте](https://cloud.yandex.ru/docs/compute/operations/image-create/custom-image) файл образа ВМ (поддерживаются образы форматов `Qcow2`, `VMDK` или `VHD`) и [загрузите](https://cloud.yandex.ru/docs/compute/operations/image-create/upload) его в бакет Yandex Object Storage, после чего он станет доступен при создании ВМ.

### Создание публичных образов

Чтобы пользователи Yandex Cloud могли создавать ВМ и диски с помощью вашего образа, нужно открыть к нему доступ и он станет публичным. Это делается не из консоли управления, а из интерфейса командной строки Yandex Cloud CLI.