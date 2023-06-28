### Convert list to set

```bash
resource "local file" "pet" {
    filename = each.value
    for_each = toset(var.filename)
}

# for_each creates a map of values, count creates list

resource "local_file" "name" {
    filename = each.value
    sensitive_content = var.content
    for_each = toset(var.users)
}

variable "users" {
    type = list(string) # will be converted to set and duplicates removed
    default = [ "/root/user10", "/root/user11", "/root/user12", "/root/user10"]
}
variable "content" {
    default = "password: S3cr3tP@ssw0rd"
  
}
```



### Example

```bash
# Предположим, надо создать несколько подсетей для нескольких зон и получить datasource подсети для определённых сетей. Сделать это можно так:

data "yandex_vpc_network" "default" {
  name = "default"
}

data "yandex_vpc_subnet" "default" {
  for_each = toset(["ru-central1-a", "ru-central1-b"])

  name = "${data.yandex_vpc_network.default.name}-${each.key}"
} 

output "yandex_vpc_subnets" {
  description = "Yandex.Cloud Subnets map"
  value       = data.yandex_vpc_subnet.default
} 

# В for_each указывается коллекция (множество или ассоциативный массив), которую нужно перебрать в цикле. name — конфиг, который мы генерируем. Внутри конфига можно использовать each.key, чтобы получить ключ текущего элемента коллекции, или each.value — для получения текущего значения элемента коллекции.

Этот конфиг даст папку с именем yandex_vpc_subnets, внутри которой будет информация о ru-central1-a и ru-central1-b

А дальше можно получить id для подсети ru-central1-a по пути yandex_vpc_subnets["ru-central1-a"].subnet_id
```

