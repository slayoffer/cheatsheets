### Install Terraform

```bash
# Скачать Terraform из зеркала Yandex.Cloud - https://hashicorp-releases.yandexcloud.net/terraform/

# После загрузки добавить путь к папке с исполняемым файлом в переменную PATH:
export PATH=$PATH:/path/to/terraform

# Указать источник установки провайдера, добавив следующую конфигурацию в файл ~/.terraformrc:
provider_installation {
  network_mirror {
    url = "https://terraform-mirror.yandexcloud.net/"
    include = ["registry.terraform.io/*/*"]
  }
  direct {
    exclude = ["registry.terraform.io/*/*"]
  }
}
```

### Install Yandex Cloud CLI

```bash
https://cloud.yandex.ru/docs/cli/operations/install-cli

curl https://storage.yandexcloud.net/yandexcloud-yc/install.sh | bash

source ~/.bashrc 

yc init

yc compute image list --folder-id standard-images|grep ubuntu-20

yc vpc subnets list
```



### IAM-токен

```bash
https://cloud.yandex.ru/docs/cli/operations/authentication/federated-user
https://cloud.yandex.ru/docs/iam/operations/iam-token/create-for-federation

# Когда вы откроете инструкцию, вам потребуется ID федерации: bpfpfctkh7focc85u9sq
yc init --federation-id=bpfpfctkh7focc85u9sq

# create token (valid 12 hours)
yc iam create-token

# Полученный IAM-токен нужно добавить в переменную окружения YC_TOKEN:
export YC_TOKEN=t1.9euelZrOx5WdjIzJkY-Sxsebi43HlO3rnpWaz4vHyJrMnZWQzsbGjZmPz5Hl8_d5bmJh-e8XOzEd_t3z9zkdYGH57xc7MR3-.4WAoNOHDF78GRpD1R86gx8iTt2oxv_0A0cD1fBUhtaMsTXhbU5pfRZOGypBj3Horf9knzE0CRQ3lEIFOJE4WDg

# Эту команду нужно выполнить перед выполнением команд terraform, если закончилось время жизни токена или если вы завершили сессию в терминале на виртуальной машине.
```



### VM config

```bash
# создайте папку example-01, а в ней — файл main.tf
vim main.tf

terraform {
  required_providers {
    yandex = {
      source  = "yandex-cloud/yandex"
    }
  }
}

provider "yandex" {
  cloud_id  = "b1g3jddf4nv5e9okle7p"
  folder_id = "b1g6kb8sqccdk2sg5drr"
  zone      = "ru-central1-b"
}

resource "yandex_compute_instance" "vm-1" {
    name = "chapter5-lesson2-std-012-30"

    resources {
        cores  = 2
        memory = 2
    }

    boot_disk {
        initialize_params {
            image_id = "fd80qm01ah03dkqb14lc"
        }
    }

    network_interface {
        subnet_id = "e2l6gdef5mmrsc7o17om"
        nat       = false
    }

    metadata = {
        ssh-keys = "ubuntu:${file("~/.ssh/id_rsa.pub")}"
    }
}

output "ip_address" {
    value = yandex_compute_instance.vm-1.network_interface.0.ip_address
}

# логин на вм под пользователем ubuntu. НУЖНО ЗАРАНЕЕ СОЗДАТЬ id_rsa на машине с терраформ

# проверка кредов (доступа)
terraform init

# проверка конфига линтером
terraform validate

# проверка dry run
terraform plan

# запуск конфига
terraform apply

# destroy vm
terraform destroy
```

### S3 bucket

```bash
# Для этого в конфигурацию terraform нужно добавить описание «бэкенда» хранения:
vim main.tf

terraform {
  required_providers {
    yandex = {
      source  = "yandex-cloud/yandex"
    }
  }

  # Описание бэкенда хранения состояния
  backend "s3" {
    endpoint   = "storage.yandexcloud.net"
    bucket     = "terraform-state-<ИМЯ СТУДЕНТА>"
    region     = "ru-central1"
    key        = "terraform.tfstate"

    skip_region_validation      = true
    skip_credentials_validation = true
  }
}

provider "yandex" {
  cloud_id  = "b1g3jddf4nv5e9okle7p"
  folder_id = "b1g10hb6v5a9qe3ut7na"
  zone      = "ru-central1-a"
}

# Создайте в Yandex Cloud Object Storage бакет с именем "terraform-state-<ИМЯ СТУДЕНТА>"

# Создайте статический ключ для сервисного аккаунта в фолдере вашей когорты. Статический ключ — это две строки: идентификатор и секрет. Добавьте эти строки в переменные окружения:
export AWS_ACCESS_KEY_ID=<идентификатор статического ключа>
export AWS_SECRET_ACCESS_KEY=<секретный ключ>

# И повторно выполните команду terraform init для инициализации нового бэкенда хранения состояния.
```

### VPC

```bash
# VPC используются для передачи информации внутри облака и соединения облачных ресурсов с интернетом

# Если в двух словах, нужно создать виртуальную машину, поместить её в подсеть, подсеть в сеть, а сеть в зону

# Для того, чтоб «подключить» сеть к ВМ, воспользуемся datasources yandex_vpc_network, а подсети — yandex_vpc_subnet.
data "yandex_vpc_network"
# Обязательные параметры
* network_id — id сети
* name — имя сети. Задать нужно или name, или network_id
# Необязательные параметры
* description — описание сети
# data "yandex_vpc_subnet"
# Обязательные параметры
* subnet_id — id подсети
# Необязательные параметры
* name — имя подсети. Задать нужно или name, или subnet_id.
```

### Custom provider

```bash
Чтобы появился свой провайдер нужно:

Написать клиентскую библиотеку к API своего сервиса, в котором мы хотим создать какие-то сущности (развернуть виртуалку, создать базу данных и другое).

Склонировать темплейт (https://github.com/hashicorp/terraform-provider-template) и переписать под использование API библиотеки.

Собрать бинарник на основе предыдущего шага. В этот бинарник потом будем скармливать конфигурационные файлы *.tf.
Положить в этот бинарник папку с плагинами Terraform (дефолтный путь для Linux ~/.terraform.d/plugins) и потом ссылаться на них в required_providers.

Бинарник обработает переданные ему конфигурационные файлы и выполнит ряд действий: создаст виртуалку, удалит её и сделать всё остальное.

Terraform «из коробки» включает ряд плагинов и берёт их из registry.terraform.io.

Если нужно подключить приватный registry или использовать зеркало.

Если интересно, как выглядят исходники провайдера для Яндекс.Облака, посмотреть можно тут:
https://github.com/yandex-cloud/terraform-provider-yandex
```

### Datasource

```bash
Datasource — это дополнительный, подключаемый источник данных.
Например, с помощью datasource мы можем получить список подсетей в сервисе и сложить их в переменную для дальнейшего использования. Так, для получения сетей и их id в Yandex.Cloud мы применяли команду yc vpc subnets list. Дальше этот id использовали при создании ВМ.
Чтобы каждый раз вручную не запрашивать id, можно написать свой datasource, который это сделает за нас.
Мы знаем, что полное имя подсети выглядит как default-ru-central1-a.
Напишем модуль tf-yc-network для получения subnet_id. Чуть позже обсудим подробнее datasource yandex_vpc_network и yandex_vpc_subnet. В main.tf после блоков terraform и provider "yandex":

variable "network_zone" {
  description = "Yandex.Cloud network availability zones"
  type        = string
  default     = "ru-central1-a"
}

data "yandex_vpc_network" "default" {
  name = "default"
}

data "yandex_vpc_subnet" "default" {
  name = "${data.yandex_vpc_network.default.name}-${var.network_zone}" 
} 

output "yandex_vpc_subnets" {
  description = "Yandex.Cloud Subnets map"
  value       = data.yandex_vpc_subnet.default
} 

Результат, полученный из datasource, можно записать в блок output и использовать дальше в других модулях по имени module.<имя модуля>.yandex_vpc_subnets.
Например, module.yandex_cloud_network.yandex_vpc_subnets, если мы потом подключим написанный модуль следующим образом:
module "yandex_cloud_network" {
    source = "./modules/tf-yc-network"
} 
Terrafrom заберёт данные из datasource и вернёт их в том формате, в котором получил из datasource. 
Дальше эти данные складываются в переменную data.<имя datasource>.<локальное имя> — у нас это data.yandex_vpc_subnet.default — и могут использоваться в конфигурации.
Кстати, вот что он узнает про наш запрос информации о сети ru-central1-a от datasource:
Changes to Outputs:
  + yandex_vpc_subnets = {
      + created_at     = "2021-10-30T05:29:08Z"
      + description    = "Auto-created default subnet for zone ru-central1-a"
      + dhcp_options   = []
      + folder_id      = "b1g10hb6v5a9qe3ut7na"
      + id             = "e9bq7u62i4q21jq25n5j"
      + labels         = {}
      + name           = "default-ru-central1-a"
      + network_id     = "enplibkhb5estnhe9l35"
      + route_table_id = ""
      + subnet_id      = "e9bd4vf8tm60md55lp0k"
      + v4_cidr_blocks = [
          + "10.128.0.0/24",
        ]
      + v6_cidr_blocks = []
      + zone           = "ru-central1-a"
    }
    
https://www.terraform.io/language/data-sources
```

