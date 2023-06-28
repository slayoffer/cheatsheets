### Generate an ssh key

```bash
ssh-keygen -t ed25519 -C "jay default"
```

### Protect key

```bash
# to get rid of "WARNING: UNPROTECTED PRIVATE KEY FILE!"
chmod 600 ~/.ssh/id_rsa
```

### Copy the ssh key to the server(s)

```bash
ssh-copy-id -i ~/.ssh/ansible_id_ed25519.pub slayo@10.0.0.9
```

### Generate an ssh key that’s going to be specifically used for Ansible

```bash
ssh-keygen -t ed25519 -C "ansible"
```

### Copy the ssh key to the server(s)

```bash
ssh-copy-id -i ~/.ssh/ansible.pub
```

### Use an SSH key to connect to a server

```bash
ssh -i .ssh/<key_name> <IP Address>
```

### To cache the passphrase for our session, we can use the ssh agent

```bash
eval $(ssh-agent)
ssh-add
```

### Here’s an alias you can put in your .bashrc, to simplify it

```bash
alias ssha='eval $(ssh-agent) && ssh-add'
```

### Disable Host Key SSH check

```bash
sudo vim /etc/ansible.cfg
# uncomment
host_key_checking=false

# or create file and add
[defaults]
host_key_checking = False
```