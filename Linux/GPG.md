### Deprecation warning fix

```bash
sudo apt-key list

#D89C 66D0 E31F EA28 74EB D205 6192 2AB6 0068 FCD6

apt-key export 0068FCD6 | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/keepassxc_key.gpg

sudo apt-key --keyring /etc/apt/trusted.gpg del 0068FCD6

sudo apt update
```

### Install

```bash
sudo apt install gnupg

sudo yum install gnupg2
```

### Generate key

```bash
gpg --gen-key

# Здесь pub – это открытый ключ, значение «FFBD0C66CBA22A8739ACB07949192168EGELF50S» — его отпечаток, uid – идентификатор, состоящий из полей имени и e-mail, пользователя-владельца ключа. Этот идентификатор нужно использовать при указании ключа, а такжедля обозначения адресата, которому предназначается сообщение (данные), зашифрованное этим ключом. Можно указывать любое из полей. В нижеследующих примерах будет указываться email. Для созданных ключей также автоматически будет сгенерирован и сертификат отзыва. Либо это можно сделать вручную:
gpg --output ~/revocation.crt --gen-revoke your_email@address.ru
```

### Sign key

```bash
# Созданный сертификат revocation.crt нужно хранить также надёжно, как и закрытые ключи. При создании сертификата отзыва нужно указать возможную причину отзыва ключа, а также комментарий к отзыву. Эту информацию будут видеть другие пользователи систем управления ключами GPG. Опция —output служит для вывода сертификата отзыва в файл, в данном случае revocation.crt. Очень важно также подписывать ключи, которые были получены от других пользователей, если достоверно известно, что это именно те пользователи, за которых они себя выдают и ключи принадлежат действительно им. Это часто происходит при импорте открытых ключей других пользователей. После проверки их (и ключей и их пользователей-владельцев) очень желательно эти ключи подписать:
gpg --sign-key other_user@address.ru
```

### Import / export key

```bash
# export
gpg –-output ~/signed.key –-export –-armor other_user@address.ru
# Опция —armor создаёт вывод в виде символов ASCII, т. е. в виде текста. Поумолчанию (если эту опцию не указывать) gpg создаст двоичный вывод. Теперь полученный файл с подписанным ключом можно отправлять его владельцу.

# import
gpg --import ~/signed.key
```

### Move key to other location

```bash
# export for moving
gpg --output mygpgkey_private.txt --armor --export-secret-key my_email.address.ru

# accept key
gpg --allow-secret-key-import --import mygpgkey_private.txt
```

### Encrypt key

```bash
gpg --recipient other_user@address.ru --encrypt somefile

# decypher key
gpg --decrypt-files somefile.gpg
```

### Check key

```bash
# check this cmd output both on owner and user pc, it has to be equal
gpg --fingerprint john@silver.ru
```

