### Install SSHFS

```bash
sudo apt install sshfs
```

### Connect remote server and mount remote folder

```bash
sshfs -o IdentityFile=/home/slayo/.ssh/yandexvm_id_ed25519 student@158.160.40.92:/home/student/documents remote_documents/
# IdentityFile for SSH key path
# remote_documents/ for local folder where to mount remote folder at
# student@158.160.40.92:/home/student/documents for remote folder
```

### Unmount remote folder

```bash
umount remote_documents/
# or
fusermount -u remote_documents/ # -u for unmount
```

