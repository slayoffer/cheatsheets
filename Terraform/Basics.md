### Help

```bash
terraform
terraform -h
terraform --help

# for cmd
terraform apply --help 
```

### Check syntax

```bash
terraform fmt-check
```

### Validate syntax

```bash
terraform validate
```

### Improve config file syntax

```basg
terraform fmt
```

### Show current state

```bash
terraform show
# for json format
terraform show -json

# for specific resource
terraform state show local_file.file
```

### List providers (plugins)

```bash
terraform providers

# change provider mirror
terraform providers mirror /dir/to/mirror/mirror_file
```

### Show output

```bash
terraform output
# specific resource value
terraform output pet-name
```

### Update state

```bash
terraform refresh
```

### Create file structure, provider and dependency graphs

```bash
sudo apt install graphviz

terraform graph | dot -Tsvg > graph.svg
```



### Popular cmds

```bash
terraform fmt # форматирует файлы Terraform в текущем каталоге

terraform init # подгружает всё необходимое для работы в текущем каталоге

terraform validate # проверяет файлы Terraform на синтаксические ошибки

terraform plan # выводит план изменений

terraform apply # применяет изменения конфигурации инфраструктуры

terraform destroy # удаляет инфраструктуру, описанную в текущем каталоге

terraform output # выводит в терминал значения переменных состояния

terraform show # отображает текущее состояние инфраструктуры

terraform state # позволяет производить манипуляции с файлами состояния

terraform version # выводит текущую версию Terraform
```

### Correct flow

```bash
terraform init
terraform validate
terraform fmt
terraform plan
terraform apply
terraform destroy
```

### Use cache

```bash
terraform plan --refresh=false
```

