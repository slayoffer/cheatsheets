### **Generate new ssh key in the ~/.ssh/ folder of the server**

```bash
# simplest way which will generate RSA key (not the most secure)
ssh-keygen

# with filename
ssh-keygen -t rsa -f ~/.ssh/ssh_key

# for most secure way
ssh-keygen -t ed25519
ssh-keygen -t ed25519 -C "email or username or server name or whatever" # -C for comment

# for old systems which dont support ed25519
ssh-keygen -b 4096
# or
ssh-keygen -t rsa -b 4096 -C "email"

-t — тип ключа
-b — длина ключа в битах (по умолчанию 3072 для RSA)
-f — путь к файлу ключа (по умолчанию ~/.ssh/id_rsa)
-C — комментарий к ключу (по умолчанию username@hostname)
-P — пароль для доступа к ключу 
```
