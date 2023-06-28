### Скопировать с удаленного сервера себе на ПК

```bash
# сперва указывается путь ОТКУДА и затем КУДА
scp	source_file target_ file

# копирование на рабочий стол (с Линукса на Винду при запуске с терминала Винды)
scp slayo@192.168.1.38:media/One.Point.O.2004.DVDRip.avi ~/Desktop

# copy to home folder of the server (:)
scp ~/test.txt slayo@192.168.1.38: # : means copy to ~ folder

# SSH key copy
scp ~/.ssh/ansible_id_ed25519 slayo@10.0.0.9:~/.ssh/
```

### С указанием ключа SSH

```bash
scp -i ~/.ssh/id_rsa testfile.txt devops@web01:/tmp/
```

### Copy folders

```bash
scp -pr /home/bob/media devappØ1:/home/bob 
# -p to preserve owner rights, -r to recursive dir copy
scp -r /home/downloads jdoe@192.168.1.38:downloads # will copy to ~/downloads

# with key
sudo scp -r  -i ~/.ssh/rubius_rsa_key ~/certs localadmin@51.250.68.145:/home/localadmin/backups
scp -i .ssh/rubius_rsa_key -r /etc/letsencrypt/ localadmin@51.250.68.145:/home/localadmin/backup/letsencrypt
```

### Change port

```bash
scp -P 2222 -rv /home/bob/media devappØ1:/home/bob # -P for port, -v for verbose
```

### Config

```bash
# for shortcuts
vim ~/.ssh/config

# for port change, etc
sudo vim /etc/sshd
```

