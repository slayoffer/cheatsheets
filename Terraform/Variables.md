### Variables

```bash
# В Terraform можно параметризовать значения и три типа переменных:
Input переменные
Output переменные
Local переменные
```

### Types

```bash
variable "name" {
     type = string
     default = "Mark"
}
variable "number" {
     type = bool
     default = true
}
variable "distance" {
     type = number
     default = 5
}
variable "jedi" {
     type = map
     default = {
     filename = "/root/first-jedi"
     content = "phanius"
     }
}

variable "gender" {
     type = list(string)
     default = ["Male", "Female"]
}
variable "hard_drive" {
     type = map
     default = {
          slow = "HHD" # to fetch - var.hard_drive["slow"]
          fast = "SSD"
     }
}
variable "users" {
     type = set(string) # set array cant have equal values in it
     default = ["tom", "jerry", "pluto", "daffy", "donald", "jerry", "chip", "dale"]
}

variable "stuff" {
     type = tuple # different types array
     default = ["tom", 2, true, false]
}

variable "rg_config" {
  type = object({
    create_rg = bool
    name      = string
    location  = string
  })
  default = {
    create_rg = true
    name      = "jane"
    location  = "Moscow"
  }
}
```

### Vars priority check

```bash
# from less important to most important
Environment Variables
terraform.tfvars
*.auto.tfvars (alphabetical order)
-var or —var-file (adhoc command-line flags)
```





### Adhoc vars input

```bash
terraform apply -var "filename=/root/pets.txt" -var "content=We love Pets!" -var "prefix=Mrs" -var "separator=." -var "length=2"

# if defaults are no set and no adhoc vars then can input vars values after terraform apply in interactive mode
```

### Env vars

```bash
# Через переменные среды. Переменная должна начинаться с TF_VAR_, а дальше уже имя переменной:
export TF_VAR_filename="/root/pets.txt"
export TF_VAR_content="We love pets!"
export TF_VAR_prefix="Mrs"
export TF_VAR_separator="."
export TF_VAR_length="2"
terraform apply


# для простого типа
export TF_VAR_instance_zone=ru-central-d
# для составного типа
export TF_VAR_instance_zone='["ru-central-a","ru-central-b"]'
```

### Vars file

```bash
# Через файл с расширением .tfvars (пачками). Создаём файл *.tfvars:
instance_zone=ru-central-b 

# these files will be auto found by terraform
terraform.tfvars
terraform.tfvars.json
*.auto.tfvars
*.auto.tfvars.json

# можно явно обозначить файл для загрузки:
terraform apply -var-file="testing.tfvars" 
```



### Set vars

```bash
# Значения переменных можно задать несколькими способами:

# Через variable или locals.

# В рабочем пространстве модуля.

# В main.tf подключим модуль с yandex_compute_instance и зададим переменную зоны:

module "yandex_cloud_instance" {
  ...
  instance_zone = var.instance_zone
  ...
} 

# Задание переменных можно использовать вместе и в любых комбинациях. Одной переменной не может быть присвоено несколько значений в одной конфигурации. Ещё больше про переменные написано тут:
https://www.terraform.io/cloud-docs/workspaces/variables
```

### Input vars

```bash
# Каждая входная переменная, принимаемая модулем, должна быть объявлена c помощью блока variable:
variable "instance_zone" {
  default     = "ru-central1-a"
  type        = string
  description = "Instance availability zone"
  validation {
    condition     = contains(toset(["ru-central1-a", "ru-central1-b", "ru-central1-c"]), var.instance_zone)
    error_message = "Select availability zone from the list: ru-central1-a, ru-central1-b, ru-central1-c."
  }
  sensitive = true
  nullable = false
}

# После ключевого слова variable идёт имя переменной и оно должно быть уникальным в одном модуле.
# В примере выше instance_zone — имя переменной.
# Имя используется для присвоения значения переменной извне и для ссылки на значение переменной внутри модуля:
default — значение по умолчанию, которое затем делает переменную необязательной.
type — какой тип должен быть у переменной. Если не указан, то тип может быть любым.
Допустимые типы string, number, bool, также есть более «сложные» типы list(<TYPE>), set(<TYPE>), map(<TYPE>) и другие, any — любой тип.

Если задано значение по умолчанию и type, то default должен соответствовать type.

description — описание переменной, своеобразная документация.

validation — блок для определения правил проверки, обычно в дополнение к ограничениям типа. В примере наложены ограничения на значение переменной zone.

sensitive — показывать или нет значение переменной в выводе apply и plan. Подробнее тут.
Любые переменные, которые зависят от sensitive, тоже будут sensitive.

nullable — определяет, может ли переменная иметь тип null. По умолчанию nullable = true. Если для какой-то переменной установлено nullable в false и определено default, то если Terraformу передадут значение этой переменной как null, он подставит дефолтное.

Для обращения к переменной в файле конфигурации можно использовать точечную нотацию var.<имя переменной>.

При создании ВМ можно задать зону вот так:
resource "yandex_compute_instance" "vm-1" {
  name = "chapter5-lesson2-<Ваш логин>"

  zone = var.instance_zone
... 
```

### Output vars

```bash
# to print output vars
terraform output
# print specific var
terraform output pet-name

Они нужны, чтобы показать информацию о вашей инфраструктуре в командной строке, а также поделиться информацией для использования другими конфигурациями Terraform. Такие переменные можно определять в output.tf

resource "random_uuid" "id7" {
}

output "id7" {
    value = random_uuid.id7.result 
}

resource "local_file" "welcome" {
    filename = "/root/message.txt"
    content = "Welcome to Kodekloud."
}

output "welcome_message" {
	value = local_file.welcome.content
	description = "bla bla"
}
```

### Local vars

```bash
# Local переменные напоминают локальные переменные в функциях. Они задаются с помощью блока locals:

locals {
  service_name = "virtual machine"
  owner        = "Y.Cloud"
} 

# Локальные переменные полезны, когда в конфигурации есть многократное повторение одних и тех же значений.
                
# Чтобы обратиться к переменной, нужно написать 
local.<имя переменной>
```