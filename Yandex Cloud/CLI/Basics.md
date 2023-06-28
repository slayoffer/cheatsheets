### Login

```bash
# via oAuth token (https://oauth.yandex.ru/authorize?response_type=token&client_id=1a6990aa636648e9b2ef855fa7bec2fb)
yc init

# via federation id
yc init --federation-id=bpfpfctkh7focc85u9sq

# check
yc config list
```

### Login config set via federation ID

```bash
yc config set federation-id bpfpfctkh7focc85u9sq
```

### Create IAM token

```bash
yc iam create-token
```

### Get images list

```bash
yc compute image list --folder-id standard-images
```

### Get subnets list

```bash
yc vpc subnets list
```



## Примеры команд

Ниже описана последовательность действий для создания облачной сети, подсети и виртуальной машины, подключенной к этой подсети.

1. Посмотрите описание команд CLI для работы с облачными сетями:

   ```
   yc vpc network --help
   ```

2. Создайте облачную сеть в каталоге, указанном в вашем профиле CLI:

   ```
   yc vpc network create \
     --name my-yc-network \
     --labels my-label=my-value \
     --description "my first network via yc"
   ```

3. Создайте подсеть в облачной сети `my-yc-network`:

   ```
   yc vpc subnet create \
     --name my-yc-subnet-a \
     --zone ru-central1-a \
     --range 10.1.2.0/24 \
     --network-name my-yc-network \
     --description "my first subnet via yc"
   ```

4. Получите список всех облачных сетей в каталоге, указанном в вашем профиле CLI:

   ```
   yc vpc network list
   
   +----------------------+------------------+-------------------------+
   |          ID          |       NAME       |       DESCRIPTION       |
   +----------------------+------------------+-------------------------+
   | skesdqhkc6449hbqqar1 | my-ui-network    | my first network via ui |
   | c6449hbqqar1skesdqhk | my-yc-network    | my first network via yc |
   +----------------------+------------------+-------------------------+
   ```

   Получите тот же список с большим количеством деталей в формате YAML:

   ```
   yc vpc network list --format yaml
   ```

   Результат:

   ```
   - id: skesdqhkc6449hbqqar1
     folder_id: ijkl9012
     created_at: "2018-09-05T09:51:16Z"
     name: my-ui-network
     description: "my first network via ui"
     labels: {}
   - id: c6449hbqqar1skesdqhk
     folder_id: ijkl9012
     created_at: "2018-09-05T09:55:36Z"
     name: my-yc-network
     description: "my first network via yc"
     labels:
       my-label: my-value
   ```

5. Создайте виртуальную машину и подключите к подсети `my-yc-subnet-a`:

   1. [Подготовьте](https://cloud.yandex.ru/docs/compute/operations/vm-connect/ssh#creating-ssh-keys) пару ключей (открытый и закрытый) для [SSH-доступа](https://cloud.yandex.ru/docs/glossary/ssh-keygen) на виртуальную машину.

   2. Создайте виртуальную машину Linux:

      ```
      yc compute instance create \
        --name my-yc-instance \
        --network-interface subnet-name=my-yc-subnet-a,nat-ip-version=ipv4 \
        --zone ru-central1-a \
        --ssh-key ~/.ssh/id_ed25519.pub
      ```

      Где

       

      ```
      ssh-key
      ```

       

      – путь к открытому ключу для SSH-доступа. В ОС виртуальной машины будет автоматически создан пользователь

       

      ```
      yc-user
      ```

       

      с указанным открытым ключом.

6. Подключитесь к виртуальной машине по SSH:

   1. Узнайте публичный IP-адрес виртуальной машины. Для этого посмотрите подробную информацию о вашей виртуальной машине:

      ```
      yc compute instance get my-yc-instance
      ```

      В выводе команды найдите адрес виртуальной машины в блоке

       

      ```
      one_to_one_nat
      ```

      :

      ```
      one_to_one_nat:
          address: 130.193.32.90
          ip_version: IPV4
      ```

   2. Подключитесь к виртуальной машине по SSH от имени пользователя

       

      ```
      yc-user
      ```

      , используя закрытый ключ:

      ```
      ssh yc-user@130.193.32.90
      ```

7. Удалите виртуальную машину `my-yc-instance`, подсеть `my-yc-subnet-a` и сеть `my-yc-network`:

   ```
   yc compute instance delete my-yc-instance
   yc vpc subnet delete my-yc-subnet-a
   yc vpc network delete my-yc-network
   ```
